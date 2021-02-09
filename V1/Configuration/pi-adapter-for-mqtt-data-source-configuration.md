---
uid: PIAdapterForMQTTDataSourceConfiguration
---

# PI Adapter for MQTT data source configuration

To use the adapter, you must configure the data source from which it polls data.

## Configure MQTT data source

**Note:** To modify MQTT data source configuration, you must use the REST endpoints to add or edit the configuration.

Complete the following steps to configure an MQTT data source:

1. Use any text editor to create a file that contains an MQTT data source in the JSON format.
    - For content structure, see [MQTT data source examples](#mqtt-data-source-examples).
    - For a table of all available parameters, see [MQTT data source parameters](#mqtt-data-source-parameters).
2. Save the file. For example, `ConfigureDataSource.json`. <!-- Not sure what the example is doing? Does it provide an example name? 
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run a PUT command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/DataSource/`.

      **Note:** The following example uses Mqtt1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration1-3).

    `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/Mqtt1/DataSource"
    ```

    **Note:** Run this command from the same directory where the file is located.

4. Configure data selection. For more information, see [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMqttDataSelectionConfiguration1-2). <!-- This link is not resolving >

## MQTT data source schema

The full schema definition for the MQTT data source configuration is in the `MQTT_DataSource_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\Mqtt\Schemas`

Linux: `/opt/OSIsoft/Adapters/Mqtt/Schemas`

## MQTT data source parameters

The following parameters are available for configuring an MQTT data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **HostNameOrIpAddress**       | Required | `string`  |  Host name or IP address of the MQTT server.  <br><br>Allowed value: any valid WS or TCP/IP endpoint address <br> Default value: `null`       |
| **Port**                      | Required | `integer` |  Port number of the MQTT  server. <br><br>Allowed value: valid port range <br> Default value: `0`. <!-- do you want that period there after "0"? >           |
| **Protocol**                  | Optional | `enum`    |   The protocol used to communicate to the MQTT broker. <br><br>Allowed value: `WS` or `TCP` <br> Default value: `TCP`         |
| **TLS**                       | Optional | `enum`    |  Determines if TLS should be used and what version. <br><br>Allowed value: none, `TLS`, `TLS11`, `TLS12`, `TLS13` <br> Default value: `TLS12`            |
| **UserName**                  | Optional | `string`  | User name or client id for accessing the MQTT server. <br><br>Allowed value: any string <br> Default value: `null`    <!-- Do we have a standard for "id"? I would always use ID unless referencing something in the GUI or in code >      |
| **Password**                  | Optional | `string`  |  Password or token for accessing the MQTT server. <br><br>**Note:** OSIsoft recommends using REST to configure the data source when the password must be specified because the adapter will encrypt the password when using REST. If you do not use REST, the plain text password will be stored on-disk. Password cannot be specified without UserName. <br><br>Allowed value: any string <br> Default value: `null`           |
| **ClientId**                  | Optional | `sting`   | Identification of the adapter component connecting to the MQTT broker.<br><br> **Note:** If null, the adapter generates a GUID.          |
| **MQTTVersion**               | Optional | `enum`    | Version  of the MQTT protocol to be used.  <br><br>Allowed value: `3.1.0`, or `3.1.1`, or `5.0.0` <br> Default value: `3.1.1`           |
| **ValidateServerCertificate** | Optional | `boolean` | Determines if server certificate gets validated. <br><br>**Note:** A warning is printed in case the server certificate validation is disabled.<br> Default value: `true`           |
| **StreamIdPrefix**            | Optional         | `string`  | Specifies what prefix is used for Stream IDs. The naming convention is `{StreamIdPrefix}{StreamId}`. An empty string means no prefix will be added to the Stream IDs and names. A `null` value defaults to _ComponentID_ followed by a period.<br><br>Example: `Mqtt1.{Topic}.{ValueField}`<br><br>**Note:** Every time you change the StreamIdPrefix of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated. <br><br>Allowed value: any string <br> Default value: `null`            |
| **DefaultStreamIdPattern**    | Optional         | `string`  |  Specifies the default stream Id pattern to use. Possible parameters: `{Topic}`, `{ValueField}`, `{DataType}`. <br><br>Allowed value: any string<br>Default value: `{Topic}.{ValueField}`           |

## Runtime changes

PI Adapter for MQTT handles runtime changes for data source configuration as follows:

When you set up a new data source configuration, the adapter automatically connects to the new data source. When you delete a data source configuration, the adapter automatically disconnects from the data source and stops data collection for all streams associated with that data source. Additionally, when you update existing data source properties, the changes are reflected at runtime.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data source configuration will be rejected.

## MQTT data source examples

The following are examples of valid MQTT data source configurations:

### Minimal data source configuration

```json
{
    "HostNameOrIpAddress" : "125.0.0.0" ,
    "Port" : 8765
}
```

### Complete data source configuration

```json
{
    "StreamIdPrefix" : null,
    "DefaultStreamIdPattern" : "{Topic}.{ValueField}",
    "HostNameOrIpAddress" : "125.0.0.0",
    "Port" : 8765,
    "Protocol" : "Tcp",
    "TLS" : "Tls12",
    "UserName": null,
    "Password": null,
    "ClientId" : "Test-Client-Id",
    "MQTTVersion" : "3.1.1",
    "ValidateServerCertificate" : true
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSource  | GET | Retrieves the MQTT data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | POST | Creates the MQTT data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | PUT | Configures or updates the MQTT data source configuration |
| api/v1/configuration/_ComponentId_/DataSource | DELETE | Deletes the MQTT data source configuration |

**Note:** Replace _ComponentId_ with the Id of your MQTT component, for example Mqtt1.
