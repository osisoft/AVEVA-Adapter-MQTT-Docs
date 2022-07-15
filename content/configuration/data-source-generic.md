---
uid: PIAdapterForMQTTDataSourceConfiguration
---

# Data source (generic)

To use the adapter, you must configure the data source from which it polls data.

## Configure MQTT data source

Complete the following steps to configure an MQTT data source. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSource` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an MQTT data source into the file.

    For sample JSON, see [MQTT data source examples](#mqtt-data-source-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [MQTT data source parameters](#mqtt-data-source-parameters).

4. Save the file. For example, as `ConfigureDataSource.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSource.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data source configuration.

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTT1/DataSource"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTT1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data source configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

7. Configure data selection.

    For more information, see [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration).

## MQTT data source schema

The full schema definition for the MQTT data source configuration is in the `MQTT_DataSource_schema.json` file located in one of the following folders:

* Windows: `%ProgramFiles%\OSIsoft\Adapters\MQTT\Schemas`
* Linux: `/opt/OSIsoft/Adapters/MQTT/Schemas`

## MQTT data source parameters

The following parameters are available for configuring an MQTT data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **HostNameOrIpAddress**       | Required | `string`  |  Host name or IP address of the MQTT server  <br><br>Allowed value: Any valid WS or TCP/IP endpoint address <br> Default value: `null`       |
| **Port**                      | Optional | `integer` |  Port number of the MQTT  server. Required only if **Protocol** is TCP. <br><br>Allowed value: Valid port range or null <br> Default value if **Protocol** is TCP: `8883` <br> Default value if **Protocol** is WS: `null`    |
| **Protocol**                  | Optional | `enum`    |   The protocol used to communicate to the MQTT broker <br><br>Allowed value: `WS` or `TCP` <br> Default value: `TCP`         |
| **TLS**                       | Optional | `enum`    |  Determines if TLS should be used and what version <br><br>Allowed value: `None`, `TLS`, `TLS11`, `TLS12`, `TLS13` <br> Default value: `TLS12`            |
| **UserName**                  | Optional | `string`  | User name or client Id for accessing the MQTT server <br><br>Allowed value: any string <br> Default value: `null`  |
| **Password**                  | Optional | `string`  |  Password or token for accessing the MQTT server <br><br>**Note:** OSIsoft recommends using REST to configure the data source when the password must be specified because the adapter will encrypt the password when using REST. If you do not use REST, the plain text password is stored on-disk. You cannot specify a Password without specifying a UserName. <br><br>Allowed value: Any string <br> Default value: `null`           |
| **ClientId**                  | Optional | `string`   | Unique identification of the adapter component connecting to the MQTT broker<br><br> **Note:** If the ClientId is null, the adapter generates a GUID.          |
| **MQTTVersion**               | Optional | `enum`    | Version  of the MQTT protocol to be used  <br><br>Allowed value: `3.1.0`, or `3.1.1`, or `5.0.0` <br> Default value: `3.1.1`           |
| **ValidateServerCertificate** | Optional | `boolean` | Determines if the server certificate gets validated <br><br>**Note:** A warning is printed in case the server certificate validation is disabled.<br> Default value: `true`           |
| **StreamIdPrefix**            | Optional         | `string`  | Specifies what prefix is used for stream Ids. The naming convention is `{StreamIdPrefix}{StreamId}`. An empty string means no prefix is added to the stream Ids and names. A `null` value defaults to _ComponentId_ followed by a period.<br><br>Example: `Mqtt1.{Topic}.{ValueField}`<br><br>**Note:** Every time you change the StreamIdPrefix of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated. <br><br>Allowed value: Any string <br> Default value: `null`            |
| **DefaultStreamIdPattern**    | Optional         | `string`  |  Specifies the default stream Id pattern to use. Possible parameters: `{Topic}`, `{ValueField}`, `{DataType}`. <br><br>Allowed value: Any string<br>Default value: `{Topic}.{ValueField}`           |
| **TimeZone** |  Optional  | `string` | Specifies the time zone associated with data received from the configured MQTT broker and its topics.<br><br>Allowed value: A valid entry from the IANA time zone database, for example `America/Los_Angeles`, `Asia/Hong_Kong`, `Europe/London`, etc. For more information, see [IANA Time Zone Database](https://www.iana.org/time-zones)<br>Default value: `null`<br><br>**Note:** Supply this field if you are using source timestamps and the timestamp does not contain time zone information.

## Runtime changes

When you set up a new data source configuration, the adapter automatically connects to the new data source. When you delete a data source configuration, the adapter automatically disconnects from the data source and stops data collection for all streams associated with that data source. Additionally, when you update existing data source properties, the changes are reflected at runtime.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data source configuration is rejected.

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
    "timeZone": "UTC",
    "streamIdPrefix": "MyPrefix.",
    "defaultStreamIdPattern": "{Topic}.{ValueField}"
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSource | `GET` | Retrieves the MQTT data source configuration. |
| api/v1/configuration/\<ComponentId\>/DataSource | `POST` | Creates the MQTT data source configuration. The adapter starts collecting data after the following conditions are met:<br/><br/>&bull; The MQTT data source configuration `POST` request is received.<br/>&bull; A MQTT data selection configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSource | `PUT` | Configures or updates the MQTT data source configuration. Overwrites any active MQTT data source configuration. If no configuration is active, the adapter starts collecting data after the following conditions are met:<br/><br/>&bull; The MQTT data source configuration `PUT` request is received.<br/>&bull; A MQTT data selection configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSource | `DELETE` | Deletes the MQTT data source configuration. After the request is received, the adapter stops collecting data. |

**Note:** Replace <ComponentId\> with the Id of your MQTT component. For example, _Mqtt1_.
