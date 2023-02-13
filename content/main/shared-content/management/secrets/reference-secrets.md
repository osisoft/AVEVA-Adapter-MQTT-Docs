---
uid: ReferenceSecrets
---

# Reference Secrets

Secrets can be referenced by their Id in any configuration through a protected property, such as a client secret or password.

## Reference Secrets by Id

To use a secret in a configuration, the value of the protected property can be replaced with `{{<secretId>}}`, where `<secretId>` is an existing id in the secret management configuration.

### Example

If a secret Id and value already exist in the secret management configuration, for example:

```code
{
    "Id": "<secretId>",
    "Value": "<secretValue>"
}
```
The secret Id can be referenced in any configuration with a protected property, such as client secret or password.

For example, in a health configuration with a PWA and AVEVA Data Hub endpoint `{{<secretId>}}` can be substituted in the protected properties:

```code
[
    {
        "Id": "AVEVA Data Hub",
        "Endpoint": "https://<AVEVA Data Hub OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "{{<secretId>}}"
    },
    {
        "Id": "PI Web API",
        "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/",
        "UserName": "<username>",
        "Password": "{{<secretId>}}"
    }
]
```

## Adding Secrets by Value

The default behavior of any configuration with a protected property is to add the value into the secrets management configuration, automatically generate a secret id, and replace the value of the protected property on the configuration to be the generated secret id.

### Example

Add a health configuration with a **ClientSecret** value `<clientsecret>`: 

```code
[
    {
        "Id": "AVEVA Data Hub",
        "Endpoint": "https://<AVEVA Data Hub OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "<clientsecret>"
    }
]
```

The secrets management configuration updates to add the following **Id** and **Value**:

```code
{
    "Id": "System.HealthEndpoints.AVEVA Data Hub.ClientSecret",
    "Value": "<clientsecret>"
}
```

 **Note:** The **Value** `<clientsecret>` is encrypted before storing in the secrets management configuration.

The health configuration replaces the **ClientSecret** value with the generated secret Id `"{{System.HealthEndpoints.AVEVA Data Hub.ClientSecret}}"`:

```code
[
    {
        "Id": "AVEVA Data Hub",
        "Endpoint": "https://<OCS OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "{{System.HealthEndpoints.AVEVA Data Hub.ClientSecret}}"
    }
]
```
