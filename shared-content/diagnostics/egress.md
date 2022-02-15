---
uid: EgressDiagnostics
---

# Egress

The Egress component of the adapter produces the following diagnostics stream:

## IO rate

The `Diagnostics.Egress.IORate` dynamic type includes the following values, which are logged in a stream with the `Id` `{machineName}.{serviceName}.OmfEgress.{EndpointId}.IORate`. `IORate` includes only sequential data successfully sent to an egress endpoint.

| Property  | Type   | Description                                            |
| --------- | ------ | -------------------------------------------------------|
|**timestamp** | `string` | Timestamp of event                                   |
| **IORate**  | `double` | One-minute rolling average of data rate (streams/second)|
