---
uid: StreamCount
---

# Stream count

The `Diagnostics.StreamCountEvent` dynamic type includes the following values, which are logged in a stream with the `Id` `{componentid}.StreamCount`. The `StreamCount` and `TypeCount` include only types and streams created for sequential data received from a data source.

| Property    | Type   | Description                                       |
| ----------- | ------ | ------------------------------------------------- |
| **timestamp** | `string` | Timestamp of event                                |
| **StreamCount** | `int`    | Number of streams created by the adapter instance |
| **TypeCount** | `int`    | Number of types created by the adapter instance   |
