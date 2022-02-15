---
uid: EgressEndpointsConfiguration
---

# Egress endpoints

PI adapters collect time series data, which  they can send to a permanent data store (endpoint). This operation is called data egress. The following endpoints are available for data egress:

- OSIsoft Cloud Services (OCS)
- PI servers through PI Web API

For long term storage and analysis, you can configure any adapter to send time series data to one or several of these endpoints in any combination. An egress endpoint is comprised of the properties specified under [Egress endpoint parameters](#egress-endpoint-parameters).

Data egress to a PI server creates a PI point in the PI adapter configuration. Data egress to OCS creates a stream in the PI adapter configuration.

The name of the PI point or OCS stream is a combination of the StreamIdPrefix specified in the adapter data source configuration and the StreamId specified in the adapter data selection configuration.

## Configure egress endpoints

Complete the following steps to configure egress endpoints. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/OmfEgress/dataendpoints` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for egress endpoints into the file.

    For sample JSON, see [Examples](#examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Egress endpoint parameters](#egress-endpoint-parameters).

4. Save the file. For example, as `ConfigureEgressEndpoints.json`.

5. Open a command line session. Change directory to the location of `ConfigureEgressEndpoints.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the egress endpoints configuration.

    ```bash
    curl -d "@ConfigureEgressEndpoints.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/OmfEgress/dataendpoints"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or replacing an egress endpoints configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## Egress endpoint configuration schema

The full schema definition for the egress endpoint configuration is in the `OmfEgress_DataEndpoints_schema.json`  file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Egress endpoint parameters

The following parameters are available for configuring egress endpoints:

| Parameter                       | Required                  | Type      | Description                                        |
|---------------------------------|---------------------------|-----------|-------------|
| **Id**                          | Optional                  | `string`    | Unique identifier<br><br>Allowed value: any string identifier<br>Default value: new GUID |
| **Endpoint**                    | Required                  | `string`    | Destination that accepts OMF v1.2 messages. Supported destinations include OCS and PI Server.<br><br>Allowed value: well-formed http or https endpoint string<br>Default: `null` |
| **Username**                    | Required for PI server endpoint  | `string`    | Basic authentication to the PI Web API OMF endpoint <br><br>_PI server:_<br>Allowed value: any string<br>Default: `null`<br>**Note:** If your username contains a backslash, you must add an escape character, for example, type `OilCompany\TestUser` as `OilCompany\\TestUser`.|
| **Password**                    | Required for PI server endpoint  | `string`    | Basic authentication to the PI Web API OMF endpoint <br><br>_PI server:_<br>Allowed value: any string<br>Default: `null`  |
| **ClientId**                    | Required for OCS endpoint | `string`    | Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null`|
| **ClientSecret**                | Required for OCS endpoint | `string`    | Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null`|
| **TokenEndpoint**               | Optional for OCS endpoint | `string`    | Retrieves an OCS token from an alternative endpoint <br><br>Allowed value: well-formed http or https endpoint string <br>Default value: `null` |
| **ValidateEndpointCertificate** | Optional                  | `boolean`   | Disables verification of destination certificate. **Note:** Only use for testing with self-signed certificates. <br><br>Allowed value: `true` or `false`<br>Default value: `true` |

### Special characters encoding

The adapter encodes special characters used in the data selection **StreamId** parameter string before sending it to configured endpoints. The encoded characters look as follows:

| Special character | Encoded character |
|-------------------|-------------------|
| &#42;             | `%2a`             |
| &#39;             | `%27`             |
| &#96;             | `%60`             |
| &#34;             | `%22`             |
| &#63;             | `%3f`             |
| &#59;             | `%3b`             |
| &#124;            | `%7c`             |
| &#92;             | `%5c`             |
| &#123;            | `%7b`             |
| &#125;            | `%7d`             |
| &#91;             | `%5b`             |
| &#93;             | `%5d`             |

## Examples

The following examples are valid egress configurations:

### Egress data to OCS

```json
[{
     "Id": "OCS",
     "Endpoint": "https://<OCS OMF endpoint>",
     "ClientId": "<clientid>",
     "ClientSecret": "<clientsecret>"
}]
```

### Egress data to PI Web API

```json
[{
     "Id": "PI Web API",
     "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/",
     "UserName": "<username>",
     "Password": "<password>"
}]
```

## REST URLs

| Relative URL                                              | HTTP verb | Action               |
|-----------------------------------------------------------|-----------|----------------------|
| api/v1/configuration/omfegress/DataEndpoints      | GET       | Gets all configured egress endpoints |
| api/v1/configuration/omfegress/DataEndpoints      | DELETE    | Deletes all configured egress endpoints |
| api/v1/configuration/omfegress/DataEndpoints      | POST      | Adds an array of egress endpoints or a single endpoint. Fails if any endpoint already exists |
| api/v1/configuration/omfegress/DataEndpoints      | PUT       | Replaces all egress endpoints |
| api/v1/configuration/omfegress/DataEndpoints      | PATCH     | Allows partial updating of configured endpoints.<br>**Note:** The request must be an array containing one or more endpoints. Each endpoint in the array must include its *Id*. |
| api/v1/configuration/omfegress/DataEndpoints/{Id} | GET       | Gets configured endpoint by *Id* |
| api/v1/configuration/omfegress/DataEndpoints/{Id} | DELETE    | Deletes configured endpoint by *Id* |
| api/v1/configuration/omfegress/DataEndpoints/{Id} | PUT       | Updates or creates a new endpoint with the specified *Id* |
| api/v1/configuration/omfegress/DataEndpoints/{Id} | PATCH     | Allows partial updating of configured endpoint by *Id* |

## Egress execution details

- After configuring an egress endpoint, egress is immediately run for that endpoint. Egress is handled individually per configured endpoint. When data is egressed for the first time, types and containers are egressed to the configured endpoint. After that only new or changed types or containers are egressed. Type creation must be successful in order to create containers. Container creation must be successful in order to egress data.
- If you delete an egress endpoint, data flow immediately stops for that endpoint. Buffered data in a deleted endpoint is permanently lost.
- Type, container, and data items are batched into one or more OMF messages when egressing. As per the requirements defined in OMF, a single message payload will not exceed 192KB in size. Compression is automatically applied to outbound egress messages. On the egress destination, failure to add a single item results in the message failing. Types, containers, and data are egressed as long as the destination continues to respond to HTTP requests.
