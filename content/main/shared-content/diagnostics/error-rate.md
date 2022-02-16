---
uid: ErrorRate
---

# Error rate

The `Diagnostics.Adapter.ErrorRate` dynamic type includes the following values, which are logged in a stream with the `Id` `{componentid}.ErrorRate`.

| Property  | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| **timestamp** | `string` | Timestamp of event                                       |
| **ErrorRate** | `double` | One-minute rolling average of error rate (streams/second)|
