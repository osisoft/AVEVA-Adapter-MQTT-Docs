---
uid: DeviceStatus
---

# Device status

The device status indicates the health of this component and if it is currently communicating properly with the data source. This time-series data is stored within a PI point or AVEVA Data Hub stream, depending on the endpoint type. During healthy steady-state operation, a value of `Good` is expected.

| Property                          | Type                                 | Description                    |
|-----------------------------------|--------------------------------------|--------------------------------|
| **Time**                        | `string`                               | Timestamp of the event        |
| **DeviceStatus**                | `string`                               | The value of the `DeviceStatus` |

The possible statuses are:

| Status                          | Meaning                               |
|-----------------------------------|---------------------------------------|
| `Good`                          | The component is connected to the data source and it is collecting data. |
| `Connected / No Data`           | The component is connected to the data source but it is not receiving data from it. |
| `Starting`                      | The component is currently in the process of starting up and is not yet connected to the data source. |
| `Device in error`               | The component encountered an error either while connecting to the data source or attempting to collect data. |
| `Shutdown`                      | The component is either in the process of shutting down or has finished. |
| `Removed`                       | The adapter component has been removed and will no longer collect data. |
| `Not Configured`                | The adapter component has been created but is not yet configured. |
