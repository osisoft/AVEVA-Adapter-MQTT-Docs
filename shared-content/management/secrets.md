---
uid: Secrets
---

# Secrets

AVEVA Adapters use secrets when authenticating with sources and destinations. All secrets are encrypted and stored in the `management_secrets.json` file and are referenced by using their ID in other configurations' protected fields (e.g. "clientSecret" : "{{Secret#2}}"). 
See [Reference Secrets](xref:ReferenceSecrets) for more information on how to use a secret Id in other configurations.

**Note:** For adapters to be as secure as possible, any secret values you configure are stored in encrypted form where cryptographic key material is stored separately in a secure location. If you edit the files directly, the adapter may not work as expected.

## Configure secrets

Complete the following steps to configure secrets. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/Management/Secrets` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for secrets into the file.

    For sample JSON, see [Example](#example).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Secrets parameters](#secrets-parameters).

4. Save the file. For example, as `ConfigureSecrets.json`.

5. Open a command line session. Change directory to the location of `ConfigureSecrets.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the secrets configuration.

    ```bash
    curl -d "@ConfigureSecrets.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/Management/Secrets"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or replacing an egress endpoints configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

On successful execution, the secrets change takes effect immediately during runtime.

## Secrets schema

The full schema definition for the egress endpoint configuration is in the `Management_Secrets_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Secrets parameters

The following parameters are available for configuring secrets:

| Parameter                 | Required | Type      | Description                                                  |
| ------------------------- | -------- | --------- | ------------------------------------------------------------ |
| **Id**              | Required | 'string' | Id of configuration to be added, edited, or removed.   Allowed value: any string that does not contain curly braces  (e.g., `<secretId>` is acceptable but `{{<secretId>}}`, `{<secretId>}`, `<secret{Id>`or `{<secretId>` are not)|
| **Description** | Optional | 'string' | Description of the secret.    Allowed value: any string   Default: `null`   **Note:** The **Description** field is metadata and not used in the adapter.|
| **ExpirationDate** | Optional | 'datetime' | Expiration date of the secret.    Allowed formats:  UTC: `"yyyy-mm-ddThh:mm:ssZ"`; Local: `"yyyy-mm-ddThh:mm:ss"`. If the time is not specified, it will default to the start of the day (e.g., `2025-06-19` will default to `2025-06-19T00:00:00`)  Default: `null`  **Note:** The **ExpirationDate** field is metadata and not used in the adapter. |
| **Value** | Required | 'string' | The secret value.   Allowed value: any string that is not explicitly enclosed by double curly braces  (e.g., `<value>`, `{{{<value>}}`, or `{{<val{ue>}}` is acceptable but `{{<value>}}` is not)|

  **Note:** If the **Value** is the masked value "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*", then the **Value** will be unchanged from the previous configuration. An error will be returned if the masked value is used when no previous configuration for that **Id** exists. 

## Example

```json
[
	{
	  "Id": "OmfEgress.DataEndpoints.Endpoint1.ClientSecret",
	  "Value": "CfDJ8FK3AvrP"
	},
	{
	  "Id": "MyCustomSecret",
	  "Description": "This is a secret",
	  "ExpirationDate": "2024-06-17",
	  "Value": "pEjrGcq7&QK6CF",
	}
]
```

## REST URLs

For the purposes of readability, the secret id is abbreviated {sid}.

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/management/secrets | GET | Returns entire secrets configuration (secret values will be hidden by *s) |
| api/v1/management/secrets | PUT | Creates or replaces entire secrets configuration |
| api/v1/management/secrets | DELETE | Deletes entire secrets configuration |
| api/v1/management/secrets/{sid} | GET | Returns a single secret if it exists (secret values will be hidden by *s) |
| api/v1/management/secrets/{sid} | PUT | Creates or replaces a single secret |
| api/v1/management/secrets/{sid} | DELETE | Deletes a single secret |

