---
uid: SchedulesConfiguration
---

# Schedules

You can configure the adapter to run scans based on a schedule. Each data item can be assigned to a schedule in the data selection configuration. The adapter samples data for those data items at the scheduled time.

**Note:** You start an ingress component without a schedule configuration, a default schedule configuration is added to use as an example.

**Note:** When the adapter framework scheduler misses or skips a scan for any reason, either one of the following messages is printed: `Scan skipped for schedule id <Id>` or `Scan missed for schedule <id>`.

## Configure schedules

Complete the following steps to change the schedules configuration:

1. Using any text editor, create a file that contains the schedules configuration in the JSON format.
    - For content structure, see the [example schedule configuration](#example-schedule-configuration).
    - For all available parameters, see the [schedules parameters](#schedules-parameters).

2. Save the file. For example, `ConfigureSchedules.json`.

3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a PUT command with the contents of the file to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/Schedules`.

    **Note:**  Replace _&lt;ComponentId&gt;_ with the ComponentId of the adapter.

    `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Note:** Run this command from the same directory where the file is located.

    ```bash
    curl -d "@ConfigureSchedules.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/<ComponentId>/Schedules"
    ```

On successful execution, the schedules change takes effect immediately during runtime.

## Schedules schema

The full schema definition for the schedules configuration is in the  `AdapterName_Schedules_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\<AdapterName>\Schemas`

Linux: `/opt/OSIsoft/Adapters/<AdapterName>/Schemas`

## Schedules parameters

The following parameters are available for configuring schedules:

| Parameter                | Required | Type      | Description |
| ------------------------ | -------- | --------- | ----------- |
|**Id**              | Required | `string` | Unique identifier for the schedule  Allowed value: any string identifier |
|**Period** | Required | `string` | The data sampling rate of the schedule. The expected format is HH:MM:SS.###.   Invalid inputs: `null`, negative timespan, or zero   A default value must be specified.|
|**Offset**     | Optional | `string` | The offset from the midnight when the schedule starts. The expected format is HH:MM:SS.###   Invalid input: negative timespan  A default value must be specified. |

**Note:** You can also specify timespans as numbers in seconds. For example, `"Period": 25` specifies 25 seconds, or `"Period": 125` specifies 2 minutes and 5 seconds.

## Example schedule configuration

The following is an example of a complete schedule configuration:

```code
[
  {
    "Id": "schedule1",
    "Period": "00:00:01.500",
    "Offset": "00:02:03"
  }
]
```

## Default schedule configuration

If no schedule is configured, the adapter uses the following default schedule configuration:

```code
[
  {
    "Id": "1",
    "Period": "0:00:05",
    "Offset": "0:00:00"
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/Schedules      | GET       | Gets all configured schedules |
| api/v1/configuration/_ComponentId_/Schedules      | DELETE    | Deletes all configured schedules |
| api/v1/configuration/_ComponentId_/Schedules      | POST      | Adds an array of schedules or a single schedule. Fails if any schedule already exists |
| api/v1/configuration/_ComponentId_/Schedules      | PUT       | Replaces all schedules |
| api/v1/configuration/_ComponentId_/Schedules/*id* | GET       | Gets configured schedule by *id* |
| api/v1/configuration/_ComponentId_/Schedules/*id*| DELETE     | Deletes configured schedule by *id* |
| api/v1/configuration/_ComponentId_/Schedules/*id* | PUT       | Replaces schedule by *id*. Fails if schedule does not exist |
| api/v1/configuration/_ComponentId_/Schedules/*id* | PATCH     | Allows partial updating of configured schedule by *id* |

**Note:** Replace *ComponentId* with the Id of your adapter component.
