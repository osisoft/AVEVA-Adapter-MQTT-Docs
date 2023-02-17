---
uid: HealthEndpointConfiguration
---

# Health endpoints

You can configure AVEVA adapters to produce and store health data at a designated health endpoint. You can use health data to ensure that your adapters are running properly and that data flows to the configured OMF endpoints.

For more information about adapter health, see [Adapter health](xref:AdapterHealth).

## Configure health endpoint

A health endpoint designates an OMF endpoint where adapter health information is sent. You can configure multiple health endpoints.

Complete the following steps to configure health endpoints. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/system/healthendpoints` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for health endpoints into the file.

    For sample JSON, see [Examples](#examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Health endpoint parameters](#health-endpoint-parameters).

4. Save the file. For example, as `ConfigureHealthEndpoints.json`.

5. Open a command line session. Change directory to the location of `ConfigureHealthEndpoints.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the health endpoint configuration.

    ```bash
    curl -d "@ConfigureHealthEndpoints.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/system/healthendpoints"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or replacing a health endpoints configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## Health endpoints schema

The full schema definition for the health endpoint configuration is in the `System_HealthEndpoints_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Health endpoint parameters

The following parameters are available for configuring health endpoints:

| Parameter                       | Required                            | Type      | Description                                        |
|---------------------------------|-------------------------------------|-----------|----------------------------------------------------|
| **Id**                          | Optional  | `string`    | A unique identifier of the endpoint configuration <br><br>Allowed value: any string identifier<br>Default value: new GUID|
| **Endpoint**                    | Required  | `string`    | The URL of a destination that accepts OMF v1.2 messages. Supported destinations include ADH and PI Server <br><br>Allowed value: well-formed http or https endpoint string<br>Default: `null`|
| **Username**                    | Optional   | `string`    | The username used for Basic authentication to the PI Web API OMF endpoint <br><br>_PI server:_<br>Allowed value: any string<br>Default: `null`|
| **Password**                    | Optional   | `string`    | The password used for Basic authentication to the PI Web API OMF endpoint <br><br>_PI server:_<br>Allowed value: any string or `{{<secretId>}}` (see [Reference Secrets](xref:ReferenceSecrets))<br>Default: `null`|
| **ClientId**                    | Required for ADH endpoint | `string`    | The clientId used for Bearer authentication to ADH endpoint <br><br>Allowed value: any string<br>Default: `null` |
| **ClientSecret**                | Required for ADH endpoint | `string`    | The clientSecret used for Bearer authentication to ADH endpoint <br><br>Allowed value: any string or `{{<secretId>}}` (see [Reference Secrets](xref:ReferenceSecrets))<br>Default: `null`|
| **DebugExpiration** | Optional | string | An optional string that enables logging of detailed information to disk for each outbound HTTP request pertaining to the egress endpoint. The value represents the date and time this detailed information should stop being saved. Examples of valid strings representing date and time:  UTC: `yyyy-mm-ddThh:mm:ssZ`, Local: `yyyy-mm-ddThh:mm:ss`. For more information, see [Egress debug logging](xref:TroubleshootTheAdapter#egress-debug-logging).<br><br>Default: `null`|
| **TokenEndpoint** | Optional | `string` | An optional token endpoint where the adapter retrieves a bearer token. When null or not specified the adapter uses a well-known Open ID URL to retrieve it <br><br>Allowed value: well-formed http or https endpoint string <br>Default value: `null` |
| **ValidateEndpointCertificate** | Optional | `boolean`  | An optional Boolean flag where, when set to false, the adapter will disable the verification of the server certificate <br><br>**Note:** AVEVA strongly recommends only disabling server certificate validation for testing purposes <br><br>Allowed value: `true` or `false`<br>Default value: `true` |

## Examples

### ADH endpoint

```code
{
    "Id": "ADH",
    "Endpoint": "https://<ADH OMF endpoint>",
    "ClientId": "<clientid>",
    "ClientSecret": "<clientsecret>"
}
```

### PI Web API endpoint

```code
{
    "Id": "PI Web API",
    "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/",
    "UserName": "<username>",
    "Password": "<password>"
}
```

### PI Web API endpoint using a valid secret Id

See [Reference Secrets](xref:ReferenceSecrets) for more information on how to use a secret Id.

```code
[{
     "Id": "PI Web API",
     "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/",
     "UserName": "<username>",
     "Password": "{{<secretId>}}"
}]
```

### PI Web API endpoint using negotiate

See [Reference Secrets](xref:ReferenceSecrets) for more information on how to use a secret Id.

```json
[{
     "Id": "PI Web API",
     "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/"
}]
```

**Note:** When you use an adapter with a PI Web API health endpoint, the AF structure is required. If the elements are deleted, the adapter recreates the elements; if the account used to authenticate to the PI Web API has its permissions removed on the AF Server, the adapter retries sending health data to the PI Web API until the permissions are restored.

## REST URLs

| Relative URL                                              | HTTP verb | Action               |
|-----------------------------------------------------------|-----------|----------------------|
| api/v1/configuration/system/healthEndpoints      | GET       | Gets all configured health endpoints |
| api/v1/configuration/system/healthEndpoints      | DELETE    | Deletes all configured health endpoints |
| api/v1/configuration/system/healthEndpoints      | POST      | Adds an array of health endpoints or a single endpoint. Fails if any endpoint already exists |
| api/v1/configuration/system/healthEndpoints      | PUT       | Replaces all health endpoints. **Note:** Requires an array of endpoints |
| api/v1/configuration/system/healthEndpoints     | PATCH     | Allows partial updating of configured health endpoints **Note:** The request must be an array containing one or more health endpoints. Each health endpoint in the array must include its *Id*.  |
| api/v1/configuration/system/healthEndpoints/*Id* | GET       | Gets configured health endpoint by *Id* |
| api/v1/configuration/system/healthEndpoints/*Id*| DELETE     | Deletes configured health endpoint by *Id* |
| api/v1/configuration/system/healthEndpoints/*Id* | PUT       | Updates or creates a new health endpoint with the specified *Id* |
| api/v1/configuration/system/healthEndpoints/*Id* | PATCH     | Allows partial updating of configured health endpoint by *Id* |

**Note:** Replace *Id* with the Id of the health endpoint.
