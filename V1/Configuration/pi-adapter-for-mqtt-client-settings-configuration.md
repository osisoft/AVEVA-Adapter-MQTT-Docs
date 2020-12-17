---
uid: PIAdapterForMQTTClientSettingsConfiguration
---

# PI Adapter for MQTT client settings configuration

The client settings configuration is automatically generated when a new data source is added. If you experience problems with timeouts or when MQTT limits are exceeded in terms of browse or subscription operation, you can change the client settings configuration.

## Configure MQTT client settings

Complete the following steps to configure the MQTT client settings:

1. Using any text editor, create a file that contains the MQTT client settings in the JSON format.
    - For content structure, see [MQTT client settings example](#mqtt-client-settings-example).
    - For all available parameters, see [MQTT client settings](#mqtt-client-settings-parameters).
2. Save the file. For example, `ConfigureClientSettings.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a `PUT` command to its appropriate endpoint:

    **Note:** The following example uses Mqtt1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    **PUT** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/ClientSettings/`

      Example using `curl`:

    ```bash
    curl -d "@ConfigureClientSettings.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/Mqtt1/ClientSettings"
    ```

    **Note:** Run this command from the same directory where the file is located.

## MQTT client settings schema

The full schema definition for the MQTT client settings configuration is in the `Mqtt_ClientSettings_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\Mqtt\Schemas`

Linux: `/opt/OSIsoft/Adapters/Mqtt/Schemas`

## MQTT client settings parameters

The following parameters are available for configuring MQTT client settings:

| Parameter                | Required | Type       | Description |
|--------------------------|----------|------------|-------------|
| **KeepAlivePeriod**      | Optional | `timespan` | Specifies keep alive period. <br><br>Allowed value: `true` or `false`<br>Default value: `true`            |
| **QosLevel**             | Optional | `enum`     |  Specifies desired QOS level:<br>At most once (0)<br>At least once (1)<br>Exactly one (2)<br><br>Default value: `2`           |
| **MaxInternalQueueSize** | Optional | `integer`  |  Specifies maximum number of value updates that can be held by the adapter in memory.<br><br>Allowed value: > `1000`<br>Default value: `500000`           |

## MQTT client settings example

```json
{
   "KeepAlivePeriod" : true,
   "QosLevel" : 2,
   "MaxInternalQueueSize" : 350000
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/ClientSettings  | GET | Retrieves the MQTT client settings configuration |
| api/v1/configuration/_ComponentId_/ClientSettings  | PUT | Configures or updates the MQTT client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | DELETE | Deletes the MQTT client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | PATCH | Allows partial updating of configured client settings fields. |

**Note:** Replace _ComponentId_ with the Id of your MQTT component, for example Mqtt1.
