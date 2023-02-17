---
uid: OnDemandHistoryRecoveryConfiguration
---

# On-demand history recovery configuration

The AVEVA adapter supports performing history recovery on-demand by specifying start and end time.

## Configure history recovery

1. Start any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests.
2. Run a `POST` command with the `Id` of the history recovery, and the `startTime` and `endTime` to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/HistoryRecoveries`.

    Example using `curl`:

    ```bash
    curl -d "{ \"Id\":\"TestRecovery\", \"startTime\":\"2021-03-29T14:00:30Z\", \"endTime\":\"2021-03-29T15:00:15Z\"  }" -X POST "http://localhost:5590/api/v1/configuration/<ComponentId>/HistoryRecoveries"
    ```

    **Note:**

    - `5590` is the default port number. If you selected a different port number, replace it with that value.
    - If you do not specify an Id, the endpoint generates a unique Id.

## History recovery parameters

Parameter | Type| Description
---------|----------|---------
 **Id** | `string` | The Id of the history recovery   **Note:** You cannot run multiple history recoveries with the same Id.
 **StartTime** | `datetime` | Time when the the first data items are collected.
 **EndTime** | `datetime`| Time when the last data items are collected.
| **Checkpoint** | `datetime` | The latest timestamp that the history recovery has completed with the range being between **startTime** and **endTime**.
| **Items** | `double` | The amount of data selection items in the history recovery operation.
| **RecoveredEvents** | `double` | Number of events that the history recovery found on the data source.
| **Progress** | `double` | Progress of the history recovery (number of data items found through the history recovery).
| **Status** | `enum` | Status of the history recovery.  The following statuses are available:  - *Active* - The operation is currently in progress - *Complete* - The operation has been completed - *Canceled* - The operation has been canceled - *Failed* - The operation failed
| **Errors** | `string` | Errors encountered during the history recovery.

## History recovery status example

```json
[
 { 
   "Id": "HistoryRecovery1", 
   "StartTime": "2021-01-09T05:55:00.0", 
   "EndTime": "2021-01-26T13:20:00.0", 
   "CheckPoint": "2021-01-13T14:55:00.0", 
   "Items": 7000, 
   "RecoveredEvents": 800000, 
   "Progress": 20, 
   "Status": "Active", 
   "Errors": null 
 }
] 
```

**Note:** The result of the history recovery operation is added to the `<componentId>_historyRecoveries.json` file.

## REST URLs

| Relative URL                                   | HTTP verb | Action |
|------------------------------------------------|-----------|--------|
| api/v1/configuration/_\<componentId>_/historyRecoveries | GET       | Returns all history recoveries statuses
| api/v1/configuration/_\<componentId>_/historyRecoveries | POST       | Initiates a new history recovery, returns the id of the operation
| api/v1/configuration/_\<componentId>_/historyRecoveries | DELETE      | Cancels all active history recovery operations and removes states
| api/v1/configuration/_\<componentId>_/historyRecoveries/_\<operationId>_ |  GET    | Gets the status of an individual history recovery
| api/v1/configuration/_\<componentId>_/historyRecoveries/_\<operationId>_ | DELETE       | Cancels history recovery and removes the state |
| api/v1/configuration/_\<componentId>_/historyRecoveries/_\<operationId>_/cancel | POST | Cancels history recovery|
| api/v1/configuration/_\<componentId>_/historyRecoveries/_\<operationId>_/resume | POST | Resumes canceled or failed history recovery operation (`202`) from the checkpoint  **Note:** If the `<operationId>` is not found, a 404 HTTP error message will be returned  | 

**Note:** Replace _\<componentId>_ with the Id of your adapter component. Replace _\<operationId>_ with the Id of the history recovery operation for which you want to perform the action.
