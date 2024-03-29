---
uid: PIAdapterForMQTTDataSelectionConfiguration
---

# Data selection (generic)

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

## Configure MQTT data selection

Complete the following steps to configure an MQTT data selection. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSelection` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an MQTT data selection into the file.

    For sample JSON, see [MQTT data selection examples](#mqtt-data-selection-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [MQTT data selection parameters](#mqtt-data-selection-parameters).

4. Save the file. For example, as `ConfigureDataSelection.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSelection.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data selection configuration.

    ```bash
    curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTT1/DataSelection"
    ```

    **Notes:**

    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTT1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data selection configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## MQTT data selection schema

The full schema definition for the MQTT data selection configuration is in the `MQTT_DataSelection_schema.json` file located in one of the following folders:

* Windows: `%ProgramFiles%\OSIsoft\Adapters\MQTT\Schemas`
* Linux: `/opt/OSIsoft/Adapters/MQTT/Schemas`

## MQTT data selection parameters

| Parameter        | Required | Type      | Description |
|------------------|----------|-----------|-------------|
| **Selected**     | Optional | `boolean` |  Selects or clears a measurement. To select an item, set the field to `true`. To remove an item, leave the field empty or set the value to `false`.  <br><br>Allowed value: `true` or `false`<br>Default value: `true`           |
| **Name**         | Optional | `string`  | The optional friendly name of the data item collected from the data source. <br><br>Allowed value: Any string<br>Default value: `null`            |
| **StreamId**     | Optional | `string`  |  The custom stream Id that is used to create the streams. If you do not specify the StreamId, the adapter generates a default stream Id based on the DataSource parameter. For more information, see [data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration#mqtt-data-source-parameters). A properly configured custom stream Id follows these rules: <br><br>Is not case-sensitive<br>Can contain spaces<br>Can contain front slashes (`/`)<br>Can contain a maximum of 100 characters<br>Cannot start with two underscores (`__`)<br>Cannot start or end with a period<br>Cannot contain consecutive periods<br>Cannot consist of only periods<br><br><br>The default Id automatically updates when there are changes to the measurement and follows the format of `<Topic>.<MetricName>`.<br><br>For more information on how the adapter encodes special characters in the **StreamId**, see [Egress endpoints](xref:EgressEndpointsConfiguration#special-characters-encoding).           |
| **DataFilterId** | Optional  | `string`  | The Id of the data filter. <br><br>Allowed value: Any string <br>Default value: `null`<br>**Note:** If the specified **DataFilterId** does not exist, unfiltered data is sent until that **DataFilterId** is created.   |
| **Topic**        | Required | `string`  |  The MQTT topic string.<br><br>Allowed value: Cannot be `null`, empty, or whitespace.        |
| **ValueField**   | Required | `string`  |  The JSONPath expression used to extract the data value from a property within the payload supplied by the MQTT server. A valid JSONPath expression starts with `$`. <sup>1</sup><br><br>Allowed value: Cannot be `null`, empty, or whitespace. For more information about JSONPath expression syntax, see <xref:JSONPathSyntaxForValueRetrievalMQTT>.          |
| **IndexField**    | Optional | `string`  | The JSONPath expression to take value to use as a timestamp from a property. A valid JSONPath expression starts with `$`. For more information about JSONPath expression syntax, see <xref:JSONPathSyntaxForValueRetrievalMQTT>. <sup>2</sup> <br><br>**Note:** The adapter generates a timestamp when `null` is specified.<br><br>Allowed value: Any valid JSONPath expression      |
| **IndexFormat**   | Optional | `string`  | The time format of the timestamp value specified in the IndexField property. <sup>2</sup><br><br>Allowed value: Any string that can be used as a DateTime format in the .NET `DateTime.TryParseExact()`method, for example `01/30/2021`.<br> For more information, see [DateTime.TryParseExact Method](https://docs.microsoft.com/en-us/dotnet/api/system.datetime.tryparseexact?view=net-5.0)<br><br>**Note:** If the string cannot be parsed, specify a custom DateTime string or one of the following keywords: `Adapter`, `UnixTimeSeconds`, `UnixTimeMilliseconds`<br>Default value: `null`            |  
| **DataType**     | Required | `string`  |  The expected data type of the values for the specified field.<br><br>Input MQTT data types are `Int16`, `Int32`, `Int64`, `UInt16`, `UInt32`, `UInt64`, `Float32`, `Float64`, `Boolean`, `String`, `Date-Time`.<br><br>Input MQTT complex data types are `Geolocation` and `Coordinates`.<br><br>For more information, see also [Principles of operation](xref:PIAdapterForMQTTPrinciplesOfOperation#data-types).|
| **DataFields** | Required | `dictionary<string, string>` | A dictionary of values with key-value pairs. The keys are specific fields for a complex type and the values are the JSONPath expression used to extract the data value from a property within the payload supplied by the MQTT server. A valid JSONPath expression starts with `$`. <sup>1</sup> <br><br>Allowed keys: `Latitude`, `Longitude`, `x`, `y`, `z`. For more information about JSONPath expression syntax, see <xref:JSONPathSyntaxForValueRetrievalMQTT>.<br>Default value: `null`

<sup>1</sup> **ValueField** and **DataFields** are mutually exclusive. For example, if you specify **ValueField**, you cannot specify **DataFields** and vice versa.

<sup>2</sup> If you do not specify **IndexField** and **IndexFormat**, the adapter automatically sets the latter to `Adapter`, which uses an adapter-supplied timestamp for the data. The timestamp is taken after the data is published to the adapter while the adapter processes it. If you specify **IndexFormat** only with a value other than `Adapter`, the validation fails and the adapter throws an error.

### CSV export/import with complex types

[EdgeCmd utility](https://docs.osisoft.com/bundle/edgecmd/page/index.html) supports the export and import of data selection with complex types, such as Geolocation and Coordinates. For example, the following command writes the data selection contents to a CSV file:

```cmd
edgecmd get DataSelection -cid MQTT1 -file <fileName>.csv -csv
```

After the CSV file is created, you can use `type <fileName>.csv` to display the contents of the file. The output will look similar to the following example in which the keywords are in the first line and the values, including complex types, are in the second line:

```cmd
topic,valueField,dataFields,indexField,dataType,indexFormat,selected,name,streamId,dataFilterId
mqtt/Boilers/Boiler1,,"{""X"":"".xValue]"",""Y"":""$.yValue"",""Z"":""$.zValue""}",$['Timestamp'],Coordinates,,True,,mqtt/Boilers/Boiler1.$.xValue.$.yValue.$.zValue,
```

## Runtime changes

When you create a new data selection item with a new **Topic** property, the adapter automatically subscribes the topic to the data source and starts data collection for the stream associated with the data selection item.  When you delete a data selection item, the adapter automatically stops collecting data for that item. Additionally, when you update a data selection item, the adapter updates the stream and continues data collection.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data selection configuration is rejected.

## MQTT data selection examples

The following are examples of valid MQTT data selection configurations<sup>1</sup>:

### Minimal data selection configuration

```json
[
  {
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "DataType" : "uint64"
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
    "ValueField" : null,
    "IndexField" : "$.Timestamp",
    "IndexFormat" : null,    
    "DataType" : "Geolocation",
    "DataFields" : { "Latitude": "$.Float1", "Longitude": "$.Float2" }
  },
  {
    "Selected" : true,
    "Name" : null,
    "StreamId" : "RandomStreamId",
    "DataFilterId" : null,
    "Topic" : "RandomTopic",
    "ValueField" : null,
    "IndexField" : "$.Timestamp",
    "IndexFormat" : null,  
    "DataType" : "Coordinates",
    "DataFields" : { "x": "$.Float4", "y": "$.Float5", "z": "$.Float6" }
  }
]
```

<sup>1</sup> **Note:** Both **ValueField** and **IndexField** require the correct structure of the JSON payload to be specified (in other words, what the data source returns). The previous examples use the following JSON payload structure:

```json
{
  "TestNode": [
    {
      "Time": "02/17/2021 12:01:36 AM PST",
      "Value": "4578"
    }
  ]
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSelection  | `GET` | Retrieves the MQTT data selection configuration, including all data selection items. |
| api/v1/configuration/\<ComponentId\>/DataSelection  | `PUT` | Configures or updates the MQTT data selection configuration. The adapter starts collecting data for each data selection item when the following conditions are met:<br/><br/>&bull; The MQTT data selection configuration `PUT` request is received.<br/>&bull; A MQTT data source configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSelection | `DELETE` | Deletes the active MQTT data selection configuration. The adapter stops collecting data. |
| api/v1/configuration/\<ComponentId\>/DataSelection | `PATCH` | Allows partial updates of configured data selection items.<br/><br/>**Note:** The request must be an array containing one or more data selection items. Each item in the array must include its **StreamId**. |
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\> | `PUT` | Updates or creates a new data selection item by **StreamId**. For new items, the adapter starts collecting data after the request is received. |
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\> | `DELETE` | Deletes a data selection item from the configuration by **StreamId**. The adapter stops collecting data for the deleted item. |

**Note:** Replace _ComponentId_ with the Id of your MQTT component. For example, _Mqtt1_.
