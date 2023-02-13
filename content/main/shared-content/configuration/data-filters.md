---
uid: DataFiltersConfiguration
---

# Data filters

AVEVA adapters can be configured to perform data filtering to save network bandwidth. Every data item in the data selection configuration can be assigned the Id of a data filter. The adapter will then filter data for those data items based on the data filter configuration.

**Note:** If data filters are enabled and data quality changes, both the old and current data quality values are passed on.

## Configure data filters

Complete the following steps to configure data filters. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/<ComponentId>/DataFilters` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for data filters into the file.

    For sample JSON, see [Data filters example](#data-filters-example).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Data filters parameters](#data-filters-parameters).

4. Save the file. For example, as `ConfigureDataFilters.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataFilters.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data filters configuration.

    ```bash
    curl -d "@ConfigureDataFilters.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/<ComponentId>/DataFilters"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or deleting a data filters configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

On successful execution, the change that you have made to data filters takes effect immediately during runtime.

## Data filters schema

The full schema definition for the data filters configuration is in the  `AdapterName_DataFilters_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Data filters parameters

The following parameters are available for configuring data filters:

| Parameter                | Required | Type      | Description |
| ------------------------ | -------- | --------- | ----------- |
|**Id**              | Required | `string` | Unique identifier for the data filter.   Allowed value: any string identifier  |
|**AbsoluteDeadband** | Optional | `double` | Specifies the absolute change in data value that should cause the current value to pass the filter test.   **Note:** You must specify `AbsoluteDeadband` or `PercentChange`.  Allowed value: double value representing absolute deadband number Default value: `null` |
|**PercentChange**     | Optional | `double` | Specifies the percent change from previous value that should cause the current value to pass the filter test.   **Note:** You must specify `AbsoluteDeadband` or `PercentChange`.  Allowed value: double value representing percent change Default value: `null` |
|**ExpirationPeriod**     | Optional | `timespan` | The length in time that can elapse after an event before automatically sending the next event, regardless of whether the next event passes the filter or not. The expected format is HH:MM:SS.### or SSS.*   Allowed value: any timespan  Default value: `null`|

**\* Note:** For example, `"ExpirationPeriod": 5:00` and `"ExpirationPeriod": 300` both specify an expiration period of 5 minutes and 0 seconds.

## Data filters example

```code
[
  {
    "Id": "DuplicateData",
    "AbsoluteDeadband": 0,
    "PercentChange": null,
    "ExpirationPeriod": "01:00:00"
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataFilters      | GET       | Gets all configured data filters. |
| api/v1/configuration/_ComponentId_/DataFilters      | DELETE    | Deletes all configured data filters. |
| api/v1/configuration/_ComponentId_/DataFilters      | POST      | Adds an array of data filters or a single data filter. Fails if any data filter already exists. |
| api/v1/configuration/_ComponentId_/DataFilters      | PUT       | Replaces all data. |
| api/v1/configuration/_ComponentId_/DataFilters      | PATCH     | Allows partial updating of configured data filter. |
| api/v1/configuration/_ComponentId_/DataFilters/*id* | GET       | Gets configured data filter by *id*. |
| api/v1/configuration/_ComponentId_/DataFilters/*id*| DELETE     | Deletes configured data filter by *id*. |
| api/v1/configuration/_ComponentId_/DataFilters/*id* | PUT       | Replaces data filter by *id*. Fails if data filter does not exist. |

**Note:** Replace *ComponentId* with the Id of your adapter component.
