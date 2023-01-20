---
uid: SystemAndAdapterConfiguration
---

# System and adapter

You can retrieve and set the configuration of the system component and adapter components together using a single file.

## Change system and adapter configuration

Complete the following steps to configure system and adapter components. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for system and adapter into the file.

    For sample JSON, see the corresponding adapter configuration examples topic.

4. Save the file. For example, as `ConfigureSystemAndAdapter.json`.

5. Open a command line session. Change directory to the location of `ConfigureSystemAndAdapter.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the system and adapter configuration.

    ```bash
    curl -d "@ConfigureSystemAndAdapter.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * In order for some of the adapter specific configurations to take effect, you have to restart the adapter.
    * Discoveries and HistoryRecoveries facet details are not required to be supplied as part of the configuration and supplied values will be ignored. 
      Their results will be restored from the previous states. 

If the operation fails due to errors in the configuration, the count of the error and suitable error messages are returned in the result.

## Retrieve system and adapter configuration

The entire application configuration can be retrieved by entering the following cURL command:

   ```bash
    curl -X GET "http://localhost:5590/api/v1/configuration"
   ```
By default, the GET response contains application configuration without the state represented by read-only configuration facets including history recoveries and discoveries. Add the "accept-verbosity" header with the value "verbose" to include read-only configuration facets in the response:

   ```bash
    curl --header "accept-verbosity: verbose" -X GET "http://localhost:5590/api/v1/configuration"
   ```
   
   **Note:** If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.

## REST URLs

| Relative URL          | HTTP verb | Action                                            |
| --------------------- | --------- | ------------------------------------------------- |
| api/v1/configuration/ | GET       | Gets the configuration for the entire application |
| api/v1/configuration/ | PUT       | Replaces the configuration for the entire application |
