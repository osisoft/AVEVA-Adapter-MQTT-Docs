---
uid: PIAdapterForMQTTClientSettingsConfiguration
---

# Client settings

The client settings configuration is automatically generated when a new data source is added. If you experience problems with timeouts or when MQTT limits are exceeded in terms of browse or subscription operation, you can change the client settings configuration.

## Configure MQTT client settings

Complete the following steps to configure MQTT client settings. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/ClientSettings` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for MQTT client settings into the file.

    For sample JSON, see [MQTT client settings example](#mqtt-client-settings-example).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [MQTT client settings parameters](#mqtt-client-settings-parameters).

4. Save the file. For example, as `ConfigureClientSettings.json`.

5. Open a command line session. Change directory to the location of `ConfigureClientSettings.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the client settings configuration.

    ```bash
    curl -d "@ConfigureClientSettings.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTT1/ClientSettings"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTT1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a client settings configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## MQTT client settings schema

The full schema definition for the MQTT client settings configuration is in the `MQTT_ClientSettings_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\MQTT\Schemas`

Linux: `/opt/OSIsoft/Adapters/MQTT/Schemas`

## MQTT client settings parameters

The following parameters are available for configuring MQTT client settings:

| Parameter     | Required | Type | Description |
|---------------|----------|------|-------------|
| **KeepAlivePeriod** | Optional | `string`<br>(timespan)          | The maximum time that can elapse between sending two data items. <br><br> Allowed value: >= `0:00:05` (5 seconds)<br>Default value: ``
| **QosLevel** | Optional |  `integer`<br><br>or<br><br>`enum`              | Defines the guarantee of delivery for a specific item.<br><br> Allowed value:<br>`0` or `AtMostOnce`: There is no guarantee of item delivery <br>`1` or `AtLeastOnce`: An item is delivered at least one time<br>`2` or `ExactlyOnce`: Each item is received only one time.<br>Default value: `` 
| **MaxInternalQueueSize**      | Optional | `integer` | Maximum number of items that can be in the adapter internal queue. <br><br>Allowed value: >= `250`<br>Default value: `` |

## MQTT client settings example

```json
{
    "keepAlivePeriod": "0:00:35",
    "qosLevel": "AtLeastOnce",
    "maxInternalQueueSize": 1000
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/ClientSettings  | GET | Retrieves the MQTT client settings configuration |
| api/v1/configuration/_ComponentId_/ClientSettings  | PUT | Configures or updates the MQTT client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | DELETE | Deletes the MQTT client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | PATCH | Allows partial updating of configured client settings fields. |

**Note:** Replace _ComponentId_ with the Id of your MQTT component, for example MQTT1.
