---
uid: DiscoveryConfiguration
---

# Discovery

You can perform a data discovery for existing data items on demand. Data discovery is initiated through REST calls and it is tied to a specific discovery Id, which you can either specify or let the adapter generate it.

Data discovery includes different routes. For example, you can choose to do the following:

- Retrieve the discovery results
- Query the discovery status
- Cancel or delete discoveries
- Merge discovery results with the data selection configuration
- Retrieve results from a current discovery and compare it with results from a previous or discovery
- Retrieve results from a current discovery and compare it with results from a current data selection configuration

## Configure discovery

1. Start any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests.
2. Run a `POST` command with the `Id` of the discovery and `autoSelect` set to either `true` or `false` to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/Discoveries`.

   **Notes:**

    * Including an `Id` is optional. If you do not include one, the adapter automatically generates one.
    * `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    ```bash
    curl -d "{ \"Id\":\"TestDiscovery\", \"autoSelect\":true }" -H "Content-Type:application/json" -X POST "http://localhost:5590/api/v1/configuration/<ComponentId>/Discoveries"
    ```

## Discovery parameters

Parameter | Type| Description
---------|----------|---------
 **id** | `string` | The Id of the discovery    **Notes:** &bull; You cannot run multiple discoveries with the same Id. &bull; Including an `id` is optional. If you do not include one, the adapter automatically generates one.
 **query** | `string` | A filter that is specific to the data source. The query filter can limit the scope of the discovery.  For more information, see the Data source configuration topic.
 **startTime** | `datetime` | Time when the discovery started
 **endTime** | `datetime`| Time when the discovery ended
 **progress** | `double` | Progress of the discovery
 **itemsFound** | `integer` | Number of data items that the discovery found on the data source
 **newItems** | `integer` | Number of new data items that the discovery found in comparison to the previous discovery
 **resultUri** | `integer` | URL at which you can access the results of the discovery
 **autoSelect** | `boolean` | When set to `true`, the result of the discovery gets pushed to the data selection configuration.
 **status** | `reference` | Status of the discovery, for example `Active` or `Complete`
 **errors** | `string`| Errors encountered during the discovery

## Discoveries status example

The following example shows the status of all discoveries. The discovery id in this example was auto-generated. 

```json
[
    {
        "id": "8ff855f1-a636-490a-bb31-207410a6e607",
        "query": null,
        "startTime": "2020-09-30T19:34:01.8180401+02:00",
        "endTime": "2020-09-30T19:34:01.8368776+02:00",
        "progress": 30,
        "itemsFound": 4,
        "newItems": 0,
        "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/<ComponentId>/Discoveries/8ff855f1-a636-490a-bb31-207410a6e607/result",
        "autoSelect": false,
        "status": "Complete",
        "errors": null
    }
]
```

## REST URLs

| Relative URL                                                                          | HTTP verb | Action                                                                                                                                  |
|---------------------------------------------------------------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------|
| api/v1/configuration/_\<componentId>_/discoveries                                        | GET       | Returns status of all discoveries                                                                                                       |
| api/v1/configuration/_\<componentId>_/discoveries                                        | POST      | Initiates a new discovery and returns its Id                                                                                            |
| api/v1/configuration/_\<componentId>_/discoveries                                        | DELETE    | Cancels and deletes all saved discoveries                                                                                               |
| api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_                          | GET       | Gets the status of an individual discovery  **Note:** If a discovery with the specified Id does not exist, you will get an error message                                                                                          |
| api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_                          | DELETE    | Cancels and deletes discovery and result                                                                                                |
| api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_/result                   | GET       | Returns the result of a discovery                                                                                                       |
| api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_/result?diff=_previousId_ | GET       | Returns the difference between the result and the previous result                                                                       |
| api/v1/configuration/_\<componentId>_/dataselection?diff=_\<discoveryId>_                   | GET       | Returns the difference between the data selection configuration and the discovery results
| api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_/result                   | DELETE    | Cancels and deletes discovery result  **Note:** The discovery Id is still valid, but a query will contain a status of `canceled` Only the **Status** property will contain a `canceled` status, but not the query |
|  api/v1/configuration/_\<componentId>_/discoveries/_\<discoveryId>_/cancel | POST | Cancels the on-demand data source discovery |
| api/v1/configuration/_\<componentId>_/dataselection/select?discoveryid=_\<discoveryId>_     | POST      | Adds the discovered items to data selection with selected set to `true`                                                                   |
| api/v1/configuration/_\<componentId>_/dataselection/unselect?discoveryid=_\<discoveryId>_   | POST      | Adds the discovered items to data selection with selected set to `false`

**Note:** Replace _\<componentId>_ with the Id of your adapter component. Replace _\<discoveryId>_ with the Id of the discovery for which you want to perform the action.
