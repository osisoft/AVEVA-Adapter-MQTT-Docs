---
uid: AdapterDiagnostics
---

# Diagnostics

The adapter and its components produce various kinds of diagnostics data that is sent to all health endpoints. The `System_Diagnostics.json` file contains a flag that determines whether diagnostics are enabled. You can change this at runtime through REST calls or the EdgeCmd utility. Diagnostics data are collected by default.

To egress diagnostics related data, you have to configure an adapter health endpoint first. See [Health endpoint configuration](xref:HealthEndpointConfiguration).

## Available diagnostics data

Every minute, dynamic data is sent to configured health endpoints.

The following diagnostics data are available:

- [System](xref:System)
- [Stream count](xref:StreamCount)
- [IO rate](xref:IORate)
- [Error rate](xref:ErrorRate)

Additionally, if the adapter supports failover, the following data is available:

- [Failover Status](xref:FailoverStatus)
