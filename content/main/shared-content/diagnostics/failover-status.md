---
uid: FailoverStatus
---

# Failover Status

The `Diagnostics.Failover.FailoverStatus` dynamic type includes the following values, which are logged in a stream with the `Id` `Failover.FailoverStatus`. This diagnostic stream contains system level information related to the failover status of the adapter.

| Property  | Type   | Description                                            |
| --------- | ------ | -------------------------------------------------------|
| **timestamp** | `string` | Timestamp of event                                    |
| **FailoverScore**  | `float` | Value between 0 and 100 indicating the adapter's overall connectivity to each data source. 100 is 'Good' and all data source connections are successful. 0 is 'Bad' and no data is being collected.|
| **FailoverRole**  | `string` | Current role of the failover instance. When the adapter assumes the `Primary` role, it is egressing data. If the adapter is set to the `Secondary` role, it does not egress data to the failover endpoint, but it may retain a cache of data if the failover mode is set to `Hot`.|