---
uid: IORate
---

# IO rate

The `Diagnostics.Adapter.IORate` dynamic type includes the following values, which are logged in a stream with the `Id` `{componentid}.IORate`. `IORate` includes only sequential data collected from a data source.

| Property  | Type   | Description                                            |
| --------- | ------ | -------------------------------------------------------|
| **timestamp** | `string` | Timestamp of event                                    |
| **IORate**  | `double` | One-minute rolling average of data rate (streams/second)|
