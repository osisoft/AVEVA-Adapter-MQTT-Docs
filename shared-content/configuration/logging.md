---
uid: LoggingConfiguration
---

# Logging

AVEVA adapters write daily log messages for the adapter, the system, and OMF egress to flat text files in the following locations:

• Windows: *%ProgramData%\OSIsoft\Adapters\{AdapterInstance}\Logs*

• Linux: */usr/share/OSIsoft/Adapters/{AdapterInstance}/Logs*

Each message in the log displays the message severity level, timestamp, and the message itself.

## Configure logging

Complete the following steps to configure logging. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/<ComponentId>/Logging` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for logging into the file.

    For sample JSON, see [Example](#example).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Logging parameters](#logging-parameters).

4. Save the file. For example, as `ConfigureLogging.json`.

5. Open a command line session. Change directory to the location of `ConfigureLogging.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the logging configuration.

    ```bash
    curl -d "@ConfigureLogging.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/<ComponentId>/Logging"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * For a list of other REST operations you can perform, like updating or retrieving a logging configuration, see [REST URLs](#rest-urls).
    * Any parameter not specified in the updated configuration file reverts to the default schema value
    <br/>
    <br/>

On successful execution, the log-level change takes effect immediately during runtime. The other configurations (log file size and file count) are updated after the adapter is restarted.

## Logging schema

The full schema definition for the logging configuration is in the component specific logging file: `AdapterName_Logging_schema.json`, `OmfEgress_Logging_schema.json`, or `System_Logging_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Logging parameters

The following parameters are available for configuring logging:

| Parameter                 | Required | Type      | Description                                                  |
| ------------------------- | -------- | --------- | ------------------------------------------------------------ |
| **LogLevel**              | Optional | reference | The logLevel sets the minimum severity for messages to be included in the logs.  Messages with a severity below the level set are not included.   The log levels in their increasing order of severity are as follows: `Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`, and `None`.  Default log level: `Information`  For detailed information about the log levels, see [LogLevel](#loglevel). |
| **LogFileSizeLimitBytes** | Optional | `integer` | The maximum size (in bytes) of log files that the service will create for the component. The value must be a positive integer.  Minimum value: `1000` Maximum value: `9223372036854775807` Default value: `34636833` |
| **LogFileCountLimit**     | Optional | `integer` | The maximum number of log files that the service will create for the component. The value must be a positive integer.  Minimum value: `1` Maximum value: `2147483647`  Default value: `31` |

### LogLevel

| Level       | Description                         |
|-------------|-------------------------------------|
| Trace         | Logs that contain the most detailed messages. These messages may contain sensitive application data like actual received values, which is why these messages should not be enabled in production environment.  **Note:** Trace is translated to *Verbose* in the log file. |
| Debug | Logs that can be used to troubleshoot data flow issues by recording metrics and detailed flow related information. |
| Information | Logs that track the general flow of the application. Any non-repetitive general information like the following can be useful for diagnosing potential application errors:   - Version information related to the software at startup   - External services used   - Data source connection string   - Number of measurements   - Egress URL   - Change of state “Starting” or “Stopping”   - Configuration |
| Warning | Logs that highlight an abnormal or unexpected event in the application flow that does not otherwise cause the application execution to stop. Warning messages can indicate an unconfigured data source state, an insecure communication channel in use, or any other event that could require attention but that does not impact data flow. |
| Error | Logs that highlight when the current flow of execution is stopped due to a failure. These should indicate a failure in the current activity and not an application-wide failure. It can indicate an invalid configuration, unavailable external endpoint, internal flow error, and so on. |
| Critical | Logs that describe an unrecoverable application or system crash or a catastrophic failure that requires immediate attention. This can indicate application wide failures like beta timeout expired, unable to start self-hosted endpoint, unable to access vital resource (for example, Data Protection key file), and so on.  **Note:** Critical is translated to *Fatal* in the log file. |
| None | Logging is disabled for the given component. |

## Example

### Default logging configuration

By default, logging captures Information, Warning, Error, and Critical messages in the message logs.
The following logging configuration is the installation default for a component:

```json
{
  "logLevel": "Information",
  "logFileSizeLimitBytes": 34636833,
  "logFileCountLimit": 31
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/System/Logging | GET | Retrieves the system logging configuration |
| api/v1/configuration/System/Logging | PUT | Updates the system logging configuration |
| api/v1/configuration/_ComponentId_/Logging | GET | Retrieves the logging configuration of the specified adapter component |
| api/v1/configuration/_ComponentId_/Logging | PUT | Updates the logging configuration of the specified adapter component |

**Note:** Replace *ComponentId* with the Id of your adapter component.
