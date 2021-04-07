---
uid: PIAdapterForMQTTSparkplugBDataSelectionConfiguration
---

# PI Adapter for MQTT Sparkplug B data selection configuration

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

## Configure MQTT Sparkplug B data selection

Complete the following steps to configure an MQTT Sparkplug B data selection. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSelection` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an MQTT Sparkplug B data selection into the file.

    For sample JSON, see [MQTT Sparkplug B data selection examples](#mqtt-sparkplug-b-data-selection-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [MQTT Sparkplug B data selection parameters](#mqtt-sparkplug-b-data-selection-parameters).

4. Save the file. For example, as `ConfigureDataSelection.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSelection.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data selection configuration.

    ```bash
    curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTTSparkplugB1/DataSelection"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTTSparkplugB1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data selection configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## MQTT Sparkplug B data selection parameters

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **Selected** | Optional | `boolean` |  Selects or clears a measurement. To select an item, set to `true`. To remove an item, leave the field empty or set to `false`.  <br><br>Allowed value: `true` or `false`<br>Default value: `true` |
| **Name** | Optional | `string` | The optional friendly name of the data item collected from the data source <br><br>Allowed value: Any string <br>Default value: `null`  |
| **Topic** | Required | `string` | The MQTT topic string in the following format: `{namespace}/{groupId}/{messageType}/{edgeNodeId}/{deviceId}`<br><br>The following rules apply to the MQTT topic string:<br>1. Both the topic string as a whole and the individual components are case-sensitive<br>2. `{namespace}` must be `spBv1.0` (case-sensitive)<br>3. `{groupId}` must be non-blank and can contain any character except for `+` and `#`<br>4. `{messageType}` must be either `DDATA` or `NDATA` (uppercase) <br>**Note:** When `NDATA` is specified, `{deviceId}` must not be specified <br>5. `edgeNodeId` must be non-blank and can contain any character except for `+` and `#`<br>6. `{deviceId}` must be non-blank and can contain any character except for `+` and `#`<br><br>**Examples**:<br>`spBv1.0/DemoCenters/DDATA/EPIC-DC004/DC004`<br>`spBv1.0/DemoCenters/NDATA/EPIC-DC004` | 
| **MetricName** | Required | `string` | The name of the property in the metric <br><br>Allowed value: Cannot be `null`, empty, or whitespace. 
| **StreamId** | Optional | `string` | The custom stream Id that is used to create the streams. If you do not specify the StreamId, the adapter generates a default stream Id based on the measurement configuration. A properly configured custom stream Id follows these rules:<br><br>Is not case-sensitive<br>Can contain spaces<br>Can contain front slashes (`/`)<br>Can contain a maximum of 100 characters<br>Cannot start with two underscores (`__`)<br>Cannot use the following characters:<br> `:` `?` `#` `[` `]` `@` `!` `$` `&` `'` `(` `)` `\` `*` `+` `,` `;` `=` `%` `<` `>` or the vertical bar<br>Cannot start or end with a period<br>Cannot contain consecutive periods<br>Cannot consist of only periods<br><br>The default Id automatically updates when there are changes to the measurement and follows the format of `<Topic>.<MetricName>`.
| **DataFilterId** | Optional | `string` | The Id of the data filter <br><br>Allowed value: Any string <br>Default value: `null`| 

## Runtime changes

When you create a new data selection item with a new **Topic** property, the adapter automatically subscribes the topic to the data source and starts data collection for the stream associated with the data selection item. When you delete a data selection item, the adapter automatically stops collecting data for that item. Additionally, when you update a data selection item, the adapter updates the stream and continues data collection.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data selection configuration is rejected.

## MQTT Sparkplug B data selection examples

The following are examples of valid MQTT Sparkplug B data selection configurations:

### Minimal data selection configuration

```json
[
  {
    "Topic" : "spBv1.0/group1/DDATA/EdgeNode1/Device1",
    "MetricName" : "Temperature"
  }
]
```

### Complete data selection configuration

```json
[
  {
    "Selected" : true,
    "Name" : "Temperature",
    "Topic" : "spBv1.0/group1/DDATA/edgeNode1/device1",
    "MetricName" : "Device1 Temperature",
    "StreamId" : "Device1.Temperature.Stream",
    "DataFilterId" : null
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSelection  | GET | Retrieves the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection  | PUT | Configures or updates the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | DELETE | Deletes the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | PATCH | Allows partial updating of configured data selection items. <br>**Note:** The request must be an array containing one or more data selection items. Each data selection item in the array must include its *StreamId*. |
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | PUT | Updates or creates a new data selection with the specified *StreamId*|
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | DELETE | Deletes a specific data selection item of the MQTT Sparkplug data selection configuration |

**Note:** Replace _ComponentId_ with the Id of your MQTT Sparkplug B component. For example, MqttSparkplugB1.
