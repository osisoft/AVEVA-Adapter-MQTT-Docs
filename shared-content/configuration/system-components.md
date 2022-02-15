---
uid: SystemComponentsConfiguration
---

# System components

PI adapters use JSON configuration files in a protected directory on Windows and Linux to store configuration that is read on startup. While the files are accessible to view, OSIsoft recommends that you use REST or the EdgeCmd utility for any changes you make to the files.

As part of making adapters as secure as possible, any passwords or secrets that you configure are stored in encrypted form where cryptographic key material is stored separately in a secure location. If you edit the files directly, the adapter may not work as expected.

**Note:** You can edit any single component or facet of the system individually using REST, but you can also configure the system as a whole with a single REST call.

## Configure system components

Complete the following steps to configure system components. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/system/components` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for system components into the file.

    For sample JSON, see [Examples](#examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [System components parameters](#system-components-parameters).

4. Save the file. For example, as `ConfigureComponents.json`.

5. Open a command line session. Change directory to the location of `ConfigureComponents.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the system components configuration.

    ```bash
    curl -d "@ConfigureComponents.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/system/components"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or deleting a system components configuration, see [REST URLs](#rest-urls).
    <br>
    <br>

## System components schema

The full schema definition for the system components configuration is in the `System_Components_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\AdapterName\Schemas`

Linux: `/opt/OSIsoft/Adapters/AdapterName/Schemas`

## System components parameters

You can configure the following parameters for system components:

| Parameters     | Required | Type    | Description |
| -------------- | -------- | --------| -------------|
| **ComponentId**    | Required |`string` | The ID of the component<sup>1</sup> . It can be any alphanumeric string. A properly configured ComponentID follows these rules:<br>Cannot contain leading or trailing space <br> Cannot use the following characters:<br> `>` `<` `/` `:` `?` `#` `[` `]` `@` `!` `$` `&` `*` `"` `(` `)` `\\` `+` `,` `;` `=` `|` `` ` `` `{` `}`<br><br>**Note:** The **ComponentId** is added to each container message that an adapter component sends to an OMF endpoint. It is displayed as the data source information (point source) in PI Web API. |
| **ComponentType**  | Required |`string` | The type of the component. There are two types of components: OmfEgress and the adapter.<sup>1</sup> |
    
<sup>1</sup>**Note:** The OmfEgress component is required to run the adapter. Both its **ComponentId** and **ComponentType** are reserved and should not be modified.

## Examples

### Default system components configuration

The default _System_Components.json_ file for the System component contains the following information.

```json
[
  {
    "ComponentId": "OmfEgress",
    "ComponentType": "OmfEgress"
  }
]
```

### System components configuration with two adapter instances

```json
[
  {
                "ComponentId": "<AdapterName>1",
                "ComponentType": "<AdapterName>"
            },
            {
                "ComponentId": "<AdapterName>2",
                "ComponentType": "<AdapterName>"
            },
            {
                "ComponentId": "OmfEgress",
                "ComponentType": "OmfEgress"
   }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/system/components | GET | Retrieves  the system components configuration |
| api/v1/configuration/system/components | POST | Adds a new component to the system configuration |
| api/v1/configuration/system/components | PUT | Updates the system components configuration |
| api/v1/configuration/system/components/_ComponentId_ | DELETE | Deletes a specific component from the system components configuration |
| api/v1/configuration/system/components/_ComponentId_ | PUT | Creates a new component with the specified *ComponentId* in the system configuration
