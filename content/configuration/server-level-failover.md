---
uid: MQTTServerLevelFailover
---

# Server-level failover

To ensure that data continues to flow in the event of a server disconnection or shutdown, you can configure server-level failover for the MQTT Adapter. This ensures a constant connection between the MQTT adapter and at least one MQTT data source at all times.

In general, MQTT Servers communicate with one another to establish, verify, and coordinate a constant, consistent, and robust connection with the MQTT adapter. MQTT Sparkplug B servers coordinate with one another according to the Sparkplug B protocol. There may be some variation on how generic MQTT servers communicate failover messages between servers and connections to clients.

## Configure a server-level failover data source

Complete the following steps to configure a server-level failover data source. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/RedundantServers` REST endpoint to initialize the configuration.

When configuring a server-level failover, you must list one (or more) data sources in the 'RedundantServers' configuration option in the MQTT or MQTTSparkplugB component.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an MQTT data source into the file.

    For sample JSON, see [Server-level data source examples](#Server-level-failover-data-source-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Server-level data source parameters](#Server-level-data-source-parameters).

4. Save the file. For example, as `ConfigureRedundantServers.json`.

5. Open a command line session. Change directory to the location of `ConfigureRedundantServers.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data source configuration.

    ```bash
    curl -d "@ConfigureRedundantServers.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTT1/RedundantServers"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTT1`, update the endpoint with your chosen component ID.

7. Configure data selection.

    For more information, see the [data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration).

## Server-level data source parameters

The following parameters are available for configuring a server-level failover data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **HostNameOrIpAddress**       | Required | `string`  |  Host name or IP address of the MQTT server.  <br><br>Allowed value: Any valid WS or TCP/IP endpoint address <br> Default value: `null`       |
| **Port**                      | Optional | `integer` |  Port number of the MQTT server. Required only if **Protocol** is TCP. <br><br>Allowed value: Valid port range or null <br> Default value if **Protocol** is TCP: `8883` <br> Default value if **Protocol** is WS: `null`    |
| **Protocol**                  | Optional | `enum`    |   The protocol used to communicate to the MQTT broker. <br><br>Allowed value: `WS` or `TCP` <br> Default value: `TCP`         |
| **TLS**                       | Optional | `enum`    |  Determines if TLS should be used and what version <br><br>Allowed value: `None`, `TLS`, `TLS11`, `TLS12`, `TLS13` <br> Default value: `TLS12`            |
| **UserName**                  | Optional | `string`  | User name or client Id for accessing the MQTT server <br><br>Allowed value: any string <br> Default value: `null`  |
| **Password**                  | Optional | `string`  |  Password or token for accessing the MQTT server <br><br>**Note:** We recommend using REST to configure the data source when the password must be specified because the adapter will encrypt the password when using REST. If you do not use REST, the plain text password is stored on-disk. You cannot specify a Password without specifying a UserName. <br><br>Allowed value: Any string <br> Default value: `null`           |
| **clientCertificateThumbprint** |  Optional | `string` | The thumbprint of the certificate when the MQTT broker requires a client certificate. <br><br> Default Value: `null` |
| **clientCertificatePassword** |  Optional | `string` | (Protected) – The password to access the certificate when the MQTT broker requires a client certificate. <br><br> Default value: `null` |
| **ValidateServerCertificate** | Optional | `boolean` | Determines if the server certificate gets validated <br><br>**Note:** A warning is printed in case the server certificate validation is disabled. <br><br> Default value: `true` |
| **serverID** |  Optional | `string`  | Unique GUID for the MQTT Server.  |

## Server level failover data source examples

The following are examples of valid MQTT server-level failover data source configurations with two redundant servers:

### Minimal server-level data source configuration

```json
{
    "HostNameOrIpAddress" : "125.1.1.1" ,
}

{
    "HostNameOrIpAddress" : "125.1.1.2" ,
}
```

### Full data source configuration

A full server configuration with two additional MQTT servers:

```json
{
  "hostNameOrIpAddress": "RedundantServer1",
  "port": "1883",
  "protocol": "Tcp",
  "tls": "None", 
  "userName": null, 
  "password": null, 
  "clientCertificateThumbprint": null, 
  "clientCertificatePassword": null, 
  "validateServerCertificate": false, 
  "serverId": "server1"
 
}

{
  "hostNameOrIpAddress": "RedundantServer2",
  "port": "1883",
  "protocol": "Tcp",
  "tls": "None", 
  "userName": null, 
  "password": null, 
  "clientCertificateThumbprint": null, 
  "clientCertificatePassword": null, 
  "validateServerCertificate": false, 
  "serverId": "server2"
}
```
