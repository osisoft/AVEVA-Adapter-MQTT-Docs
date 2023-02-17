---
uid: ClientFailoverConfiguration
---

# Client failover configuration

You can configure the adapter for failover by using client failover to register the adapter to a failover group managed by a failover endpoint. When you register the adapter to a failover group it allows the adapter to work with other adapter instances in the group as mutual backups. This minimizes the likelihood of data loss if an adapter in the group goes offline due to incidences like scheduled maintenance or an unexpected power outage.

Using client failover, you can do the following:

- Register the adapter instance to a failover group managed by a failover endpoint.
- Unregister the adapter instance from a failover group managed by a failover endpoint.
- Perform runtime failover parameter changes such as the failover mode and failover timeout.
- Query the current failover state including the failover role, last data process time, failover status and adapter state.

**Note:** Failover group will be created by the adapter if it does not exist on the failover endpoint.

## Configure client failover

Complete the following steps to configure client failover.

1. Using a text editor, create a file that contains the client failover configuration in the JSON format.
    - For sample JSON, see the [example client failover configuration](#example-client-failover-configuration).
    - For all available parameters, see the [client failover parameters](#client-failover-parameters).

2. Save the file. For example, `ConfigureClientFailover.json`.

3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a PUT command with the contents of the file to the following endpoint: `http://localhost:5590/api/v1/configuration/System/ClientFailover`.

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Note:** Run this command from the same directory where the file is located.

    ```bash
    curl -d "@ConfigureClientFailover.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/System/ClientFailover"
    ```

On successful execution, the client failover change takes effect immediately during runtime.

## Client failover parameters

| Parameter | Required | Type | Description
---------|---------|----------|---------
| **FailoverGroupId** | Required | `string` | The ID of the failover group to register the adapter instance in <br><br>Allowed value: any string identifier<br>Default value: `null` |
| **Name** | Optional | `string` | The friendly name of the failover group <br><br>Allowed value: any string value<br>Default value: `null` |
| **Description** | Optional | `string`| The description of the failover group <br><br>Allowed value: any string value<br>Default value: `null` |
| **FailoverTimeout** | Required | `datetime` | The failover timeout value of the failover group. This defines how frequently the adapter will send a heartbeat to the failover endpoint. The heartbeat will be sent at every FailoverTimeout / 2 interval <br><br>Allowed value: a string representation of date time using `hh:mm:ss` <br>|
| **Mode** | Required | `string` | The failover mode of the registered adapter <br><br>Allowed value: `Hot`, `Warm`, `Cold` <br> For more information, see [Failover Modes](#failover-modes). |
| **Endpoint** | Required | `string` | The URL of a destination that supports client failover registration. Supported destinations include ADH and on-premise Failover Service <br><br>Allowed value: well-formed http or https endpoint string<br>Default: `null` |
| **UserName** | Optional | `string` | The username used for Basic authentication to on-premise Failover Service endpoint <br><br>Allowed value: any string<br>Default: `null`<br><br>**Note:** If your username contains a backslash, you must add an escape character, for example, type `OilCompany\TestUser` as `OilCompany\\TestUser`. |
| **Password** | Optional | `string` | The password used for Basic authentication to on-premise Failover Service endpoint <br><br>Allowed value: any string or `{{<secretId>}}` (see [Reference Secrets](xref:ReferenceSecrets))<br>Default: `null` |
| **ClientId** | Required for ADH endpoint | `string` | The clientId used for Bearer authentication to ADH endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null` |
| **ClientSecret** | Required for ADH endpoint | `string`| The clientSecret used for Bearer authentication to ADH endpoint <br><br>Allowed value: any string or `{{<secretId>}}` (see [Reference Secrets](xref:ReferenceSecrets))<br>Default: `null` |
| **TokenEndpoint** | Optional | `string`| An optional token endpoint where the adapter retrieves a bearer token. When null or not specified the adapter uses a well-known Open ID URL to retrieve it <br><br>Allowed value: well-formed http or https endpoint string <br>Default value: `null` |
| **ValidateEndpointCertificate** | Optional | `boolean`| An optional Boolean flag where, when set to false, the adapter will disable the verification of the server certificate <br><br>**Note:** AVEVA strongly recommends only disabling server certificate validation for testing purposes. <br><br>Allowed value: `true` or `false`<br>Default value: `true` |

**Note:** Failover group name, description and failover timeout cannot be changed once created. To change it the group must be deleted on the failover service side.

### Failover Modes

The failover behavior outlined below corresponds to adapter instances with the 'Secondary' failover role. Available failover modes may vary based on adapter. For more information on failover role, see [Failover Role](#failover-role).

| Mode | Description
---------|---------
| **Hot** | When in `Hot` failover mode, configured components for the `Secondary` adapter instance start and collect data from the data source. Collected data is buffered into the failover-specific buffer folder until the adapter instance in the `Primary` role has finished sending data to the destination. Data from the `Secondary` adapter instance is not egressed to the data endpoint(s). |
| **Warm** | When in `Warm` failover mode, configured components for the `Secondary` adapter instance start and connect to the data source but do not collect data from the data source. Since data is not being collected, data is not buffered nor egressed to the data endpoint(s). |
| **Cold** | When in `Cold` failover mode, none of the configured components for the `Secondary` adapter instance start. The adapter does not connect to nor collect data from the data source, and data is not egressed to the data endpoint(s). |

## Example client failover configuration

The following is an example of a complete client failover configuration.

```json
 {
   "FailoverGroupId": "FailoverGroup1",
   "Name": "NameExample",
   "Description": "DescriptionExample",
   "FailoverTimeout": "00:01:00",
   "Mode": "hot",
   "Endpoint": "http://test-endpoint.com",
   "UserName": "UserName1",
   "Password": "Password1",
   "TokenEndpoint": null,
   "ValidateEndpointCertificate": true
 }
```

**Note:** When On Demand history recovery is required a new AVEVA Adapter instance should be configured to perform the operation. It isn't recommended to use On Demand history recovery on an Adapter instance that participates in a failover pair. 

## Query current failover state

You can query the current failover state with the adapter's diagnostics. Follow the instruction below to query the current failover state:
 
Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a GET command to the following endpoint: `http://localhost:5590/api/v1/diagnostics/FailoverState`.

The following is an example of failover state returned from the adapter:

```json
{
    "Role": "Primary",
    "LastDataProcessedTime": "2021-01-01T00:00:00",
    "FailoverScore": "95",
    "AdapterState": "Running"
}
```

### Failover Role 

The current failover `Role` is determined by the client failover endpoint. The current failover role is visible by querying the failover state, or by looking at the failover status diagnostics streams. For more information on failover status, see [Failover Status](xref:FailoverStatus).

| Role | Description
---------|---------
| **Primary** | When the adapter is in the `Primary` role, configured components start, collect, and egress data from the data source to the data endpoint(s). <br><br>**Note:** While the adapter is in the `Primary` role, a change in `Mode` in the client failover configuration does not affect adapter behavior and data will continue to be egressed. |
| **Secondary** | When the adapter is in the `Secondary` role, adapter behavior varies based on the failover mode. For more information, see [Failover Modes](#failover-modes). |

When an adapter with a valid client failover configuration registers with an endpoint and it is the only adapter registered in the group, it becomes the `Primary` adapter instance. If the adapter is not the only adapter registered in the client failover group, the `Primary` adapter instance is that with the highest `FailoverScore` value. For more information on `FailoverScore`, see [Failover Status](xref:FailoverStatus).

## Health

If the adapter has health endpoints configured, the client failover configuration values `Mode` and `FailoverGroupId` are included in the static failover health data. For more information, see [Failover Health](xref:FailoverHealth).

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/System/ClientFailover      | GET       | Gets the client failover configuration |
| api/v1/configuration/System/ClientFailover      | DELETE    | Deletes the client failover configuration
| api/v1/configuration/System/ClientFailover      | POST      | Creates a client failover configuration. Fails if the client failover configuration already exists |
| api/v1/configuration/System/ClientFailover      | PATCH      | Partially updates existing client failover configuration |
| api/v1/configuration/System/ClientFailover      | PUT       | Replaces the existing client failover configuration |
| api/v1/diagnostics/FailoverState                | GET     | Get the current failover state |
