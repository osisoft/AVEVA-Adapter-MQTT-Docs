---
uid: PIAdapterForMQTTSparkplugBDataSelectionConfiguration
---

# PI Adapter for MQTT Sparkplug B data selection configuration

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

## Configure MQTT Sparkplug B data selection

**Note:** You cannot modify MQTT Sparkplug B data selection configurations manually. You must use the REST endpoints to add or edit the configuration.

Complete the following steps to configure the MQTT Sparkplug B data selection:

1. Use any text editor to create a file that contains an MQTT Sparkplug B data selection in the JSON format.
    - For content structure, see [MQTT Sparkplug B data selection examples](#mqtt-sparkplug-b-data-selection-examples).
    - For a table of all available parameters, see [MQTT Sparkplug B data selection parameters](#mqtt-sparkplug-b-data-selection-parameters).
2. Save the file. For example, `ConfigureDataSelection.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run either a POST or PUT command to their appropriate endpoint:

    **Note:** The following examples use MqttSparkplugB1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration1-3).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    - **POST** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/DataSelection/`

      Example using `curl`:

      ```bash
      curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X POST "http://localhost:5590/api/v1/configuration/MqttSparkplugB1/DataSelection"
      ```

      **Note:** Run this command from the same directory where the file is located.

    - **PUT** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/DataSelection/<StreamId>`

      Example using `curl`:

        ```bash
        curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MqttSparkplugB1"
        ```

        **Note:** Run this command from the same directory where the file is located.

## MQTT Sparkplug B data selection schema

The full schema definition for the MQTT Sparkplug B data selection configuration is in the `MqttSparkplugB_DataSelection_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\MqttSparkplugB\Schemas`

Linux: `/opt/OSIsoft/Adapters/MqttSparkplugB/Schemas`

## MQTT Sparkplug B data selection parameters

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **Selected** | Optional | `boolean` |  Selects or clears a measurement. To select an item, set to `true`. To remove an item, leave the field empty or set to `false`.  <br><br>Allowed value: `true` or `false`<br>Default value: `true` |
| **Name** | Optional | `string` | The optional friendly name of the data item collected from the data source. <br><br>Allowed value: any string <br>Default value: `null`  |
| **Topic** | Required | `string` | The MQTT topic string in the following format: `{namespace}/{groupId}/{messageType}/{edgeNodeId}/{deviceId}`<br><br>The following rules apply to the MQTT topic string:<br>1. `{namespace}` must be `spBv1.0` (case-sensitive)<br>2. `{groupId}` must be non-blank and can contain any character except for `+` and `#`<br>3. `{messageType}` must be either `DDATA` or `NDATA` (uppercase) <br>**Note:** When `NDATA` is specified, `{deviceId}` must not be specified <br>4. `edgeNodeId` must be non-blank and can contain any character except for `+` and `#`<br>5. `{deviceId}` must be non-blank and can contain any character except for `+` and `#`<br><br>**Examples**:<br>`spBv1.0/DemoCenters/DDATA/EPIC-DC004/DC004`<br>`spBv1.0/DemoCenters/NDATA/EPIC-DC004` | 
| **MetricName** | Required | `string` | The name of the property in the metric. <br><br>Allowed value: cannot be `null`, empty, or whitespace. 
| **StreamId** | Optional | `string` | The custom stream ID that is used to create the streams. If you do not specify the StreamID, the adapter generates a default stream ID based on the measurement configuration. A properly configured custom stream ID follows these rules:<br><br>Is not case-sensitive.<br>Can contain spaces.<br>Cannot start with two underscores ("__").<br>Can contain a maximum of 100 characters.<br>Cannot use the following characters:<br> `/` `:` `?` `#` `[` `]` `@` `!` `$` `&` `'` `(` `)` `\` `*` `+` `,` `;` `=` `%` `<` `>` or the vertical bar<br>Cannot start or end with a period.<br>Cannot contain consecutive periods.<br>Cannot consist of only periods.<br><br>The default ID automatically updates when there are changes to the measurement and follows the format of `<Topic>.<MetricName>`
| **DataFilterId** | Optional | `string` | The ID of the data filter. <br><br>Allowed value: any string <br>Default value: `null`| 

<!--
Thyag: We might want to emphasize that the Topic as a whole is case sensitive along with its idividual components.
-->

## Runtime changes

PI Adapter for MQTT handles runtime changes for data selection configuration as follows:

When you create a new data selection item with a new **Topic** property, the adapter automatically subscribes the topic to the data source and starts data collection for the stream associated with the data selection item. When you delete a data selection item, the adapter automatically stops collecting data for that item. Additionally, when you update a data selection item, the adapter updates the stream and continues data collection.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data selection configuration will be rejected.

## MQTT Sparkplug B data selection examples

The following are examples of valid MQTT Sparkplug B data selection configurations:

### Minimal data selection configuration

```json
[
  {
    "Topic" : "spBv1.0/group1/DDATA/edgeNode1/device1",
    "MetricName" : "Temperature"
  }
]
```
<!--
Thyag: NodeIds (EdgeNode1) and DeviceIds (Device1) are capitalized by convention so we could have it here like that
-->
### Complete data selection configuration

```json
[
  {
    "Selected" : true,
    "Name" : "Temperature",
    "Topic" : "spBv1.0/group1/DDATA/edgeNode1/device1",
    "MetricName" : "Temperature",
    "StreamId" : "RandomStreamId",
    "DataFilterId" : null
  }
]
```
<!--
Thyag: Name can be specified as "Device1 Temperature" and StreamId as "Device1.Temperature.Stream" 
for better clarity from the example
-->
## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSelection  | GET | Retrieves the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection  | PUT | Configures or updates the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | DELETE | Deletes the MQTT Sparkplug B data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | PATCH | Allows partial updating of configured data selection items. <br>**Note:** The request must be an array containing one or more data selection items. Each data selection item in the array must include its *StreamId*. |
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | PUT | Updates or creates a new data selection with the specified *StreamId*|
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | DELETE | Deletes a specific data selection item of the MQTT Sparkplug data selection configuration |

**Note:** Replace _ComponentId_ with the Id of your MQTT Sparkplug B component, for example MqttSparkplugB1.
