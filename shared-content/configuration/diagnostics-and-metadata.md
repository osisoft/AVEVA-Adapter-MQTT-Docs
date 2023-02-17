---
uid: GeneralConfiguration
---

# Diagnostics and metadata

You can configure AVEVA adapters to produce and store diagnostics data at a designated health endpoint, and to send metadata for created streams.

For more information about available diagnostics data, see [Adapter diagnostics](xref:AdapterDiagnostics) and [Egress diagnostics](xref:EgressDiagnostics).
For more information about available metadata and what metadata are sent per metadata level, see [Adapter Metadata](xref:AdapterMetadata).

## Configure general

1. Start any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests.
2. Run a `PUT` command to the following endpoint, setting `EnableDiagnostics` to either `true` or `false`, `MetadataLevel` to `None`, `Low`, `Medium`, or `High` and `HealthPrefix` to a string or `null`: `http://localhost:5590/api/v1/configuration/system/general`

   **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

   Example using `curl`:

   ```bash
   curl -d "{ \"EnableDiagnostics\":true, \"MetadataLevel\":Medium, \"HealthPrefix\":\"Machine1\" }" -X PUT "http://localhost:5590/api/v1/configuration/system/general"
   ```

## General schema

The full schema definition for the general configuration is in the `System_General_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## General parameters

The following parameters are available for configuring general:

| Parameter             | Required | Type    | Description |
| ---------             | -------- | ------- | ----------- |
| **EnableDiagnostics** | Optional | `boolean` | Determines if diagnostics are enabled  Allowed value: `true` or `false` Default value: `true` |
| **MetadataLevel** | Optional | `reference` | Defines amount of metadata sent to OMF endpoints.   Allowed value: `None`, `Low`, `Medium`, and `High`  Default value: `Medium`|
| **HealthPrefix** | Optional | `reference` | Prefix to use for health and diagnostics stream and asset IDs.  Default value: `null`|

## Example

### Retrieve the general configuration

Example using `curl`:

```bash
curl -X GET "http://localhost:{port}/api/v1/configuration/system/general"
```

Sample output:

```code
{
  "EnableDiagnostics": true,
  "MetadataLevel": "Medium",
  "HealthPrefix": "Machine1"
}
```

## REST URLs

| Relative URL                            | HTTP verb | Action                                          |
| --------------------------------------- | --------- | ----------------------------------------------- |
| api/v1/configuration/system/General  | GET       | Gets the general configuration             |
| api/v1/configuration/system/General  | PUT       | Replaces the existing general configuration |
| api/v1/configuration/system/General  | PATCH       | Allows partial updating of general configuration
