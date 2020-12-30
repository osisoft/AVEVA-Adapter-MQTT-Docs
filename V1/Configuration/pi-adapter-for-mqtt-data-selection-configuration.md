---
uid: PIAdapterForMQTTDataSelectionConfiguration
---

# PI Adapter for MQTT data selection configuration

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

## Configure MQTT data selection

**Note:** You cannot modify MQTT data selection configurations manually. You must use the REST endpoints to add or edit the configuration.

Complete the following steps to configure the MQTT data selection:

1. Use any text editor to create a file that contains an MQTT data selection in the JSON format.
    - For content structure, see [MQTT data selection examples](#mqtt-data-selection-examples).
    - For a table of all available parameters, see [MQTT data selection parameters](#mqtt-data-selection-parameters).
2. Save the file. For example, `ConfigureDataSelection.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run either a POST or PUT command to their appropriate endpoint:

    **Note:** The following examples use Mqtt1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration1-3).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    - **POST** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/DataSelection/`

      Example using `curl`:

      ```bash
      curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X POST "http://localhost:5590/api/v1/configuration/Mqtt1/DataSelection"
      ```

      **Note:** Run this command from the same directory where the file is located.

    - **PUT** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/DataSelection/<StreamId>`

      Example using `curl`:

        ```bash
        curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/Mqtt1/DataSelection"
        ```

        **Note:** Run this command from the same directory where the file is located.

## MQTT data selection schema

The full schema definition for the MQTT data selection configuration is in the `Mqtt_DataSelection_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\Mqtt\Schemas`

Linux: `/opt/OSIsoft/Adapters/Mqtt/Schemas`

## MQTT data selection parameters

| Parameter        | Required | Type      | Description |
|------------------|----------|-----------|-------------|
| **Selected**     | Optional | `boolean` |  Selects or clears a measurement. To select an item, set to `true`. To remove an item, leave the field empty or set to `false`.  <br><br>Allowed value: `true` or `false`<br>Default value: `true`           |
| **Name**         | Optional | `string`  | The optional friendly name of the data item collected from the data source. <br><br>Allowed value: any string<br>Default value: `null`            |
| **StreamId**     | Optional | `string`  |  The custom stream ID that is used to create the streams. If you do not specify the StreamID, the adapter generates a default stream ID based on the DataSource parameter. For more information, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration#mqtt-data-source-parameters). A properly configured custom stream ID follows these rules:<br><br>Is not case-sensitive.<br>Can contain spaces.<br>Cannot start with two underscores ("__").<br>Can contain a maximum of 100 characters.<br>Cannot use the following characters:<br> `/` `:` `?` `#` `[` `]` `@` `!` `$` `&` `'` `(` `)` `\` `*` `+` `,` `;` `=` `%` `<` `>` or the vertical bar<br>Cannot start or end with a period.<br>Cannot contain consecutive periods.<br>Cannot consist of only periods.<br><br>The default ID automatically updates when there are changes to the measurement and follows the format of `<Topic>.<MetricName>`           |
| **Topic**        | Required | `string`  |  The MQTT topic string.<br><br>Allowed value: cannot be `null`, empty, or whitespace.        |
| **ValueField**   | Required | `string`  |  The JsonPath expression to take value from a property. A valid JsonPath expression starts with `$`.<br><br>Allowed value: cannot be `null`, empty, or whitespace.          |
| **TimeField**    | Optional | `string`  | The JsonPath expression to take value to use as a timestamp from a property. A valid JsonPath expression starts with `$`. <br>**Note:** The adapter generates a timestamp when `null` is specified.<sup>1</sup><br><br>Allowed value: any string      |
| **DataType**     | Required | `string`  |  The expected data type of the values for the specified field. <br><br>Allowed value: OMF supported data types           |
| **TimeFormat**   | Optional | `string`  | The time format of the timestamp value specified in the TimeField property.<br><br>Allowed value: any string that can be used as a DateTime format in the .NET `DateTime.TryParseExact()`method. For more information, see [DateTime.TryParseExact Method](https://docs.microsoft.com/en-us/dotnet/api/system.datetime.tryparseexact?view=net-5.0)<sup>1</sup><br><br>**Note:** If the string cannot be parsed, specify a custom datetime string or one of the following keywords: `Adapter`, `UnixTimeSeconds`, `UnixTimeMilliseconds`<br>Default value: `null`            |
| **DataFilterId** | Optional  | `string`  | The ID of the data filter. <br><br>Allowed value: any string <br>Default value: `null`            |

<sup>1</sup> If you do not specify **TimeField** and **TimeFormat**, the adapter automatically sets the latter to `Adapter`, which uses an adapter-supplied timestamp for the data. The timestamp is taken after the data is published to the adapter while the adapter processes it. If you specify **TimeFormat** only, with a value other than `Adapter`, the validation fails and the adapter throws an error.

## Runtime changes

PI Adapter for MQTT handles runtime changes for data selection configuration as follows:

When you create a new data selection item with a new **Topic** property, the adapter automatically subscribes the topic to the data source and starts data collection for the stream associated with the data selection item.  When you delete a data selection item, the adapter automatically stops collecting data for that item. Additionally, when you update a data selection item, the adapter updates the stream and continues data collection.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data selection configuration will be rejected.

## MQTT data selection examples

The following are examples of valid MQTT data selection configurations:

### Minimal data selection configuration

```json
[
  {
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "DataType" : "uint64",
  }
]
```

### Complete data selection configuration

```json
[
  {
    "Selected" : true,
    "Name" : null,
    "StreamId" : "RandomStreamId",
    "DataFilterId" : null,
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "TimeField" : "$.TestNode[:1].Time",
    "DataType" : "uint64",
    "TimeFormat" : null

  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSelection  | GET | Retrieves the MQTT data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection  | PUT | Configures or updates the MQTT data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | DELETE | Deletes the MQTT data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | PATCH | Allows partial updating of configured data selection items. <br>**Note:** The request must be an array containing one or more data selection items. Each data selection item in the array must include its *StreamId*. |
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | PUT | Updates or creates a new data selection with the specified *StreamId*|
| api/v1/configuration/_ComponentId_/DataSelection/_StreamId_ | DELETE | Deletes a specific data selection item of the MQTT data selection configuration |

**Note:** Replace _ComponentId_ with the Id of your MQTT component, for example Mqtt1.
