---
uid: AutomaticHistoryRecovery
---

# Automatic history recovery

Besides on-demand history recovery, the AVEVA adapter also supports automatic history recovery.

For automatic history recovery, the adapter tracks changes to the **DeviceStatus** of each component. When the **DeviceStatus** changes to `DeviceInError` or `Shutdown`, the adapter starts a new [History recovery interval](#history-recovery-intervals). When the issue resolves or if the adapter is restarted and the **DeviceStatus** changes to `Good`, the adapter closes any current intervals for that component. The adapter tracks these intervals for each component and, when DeviceStatus has a value of `Good`, it performs history recovery for these intervals starting from oldest to newest. For more information, see also [Device status](xref:DeviceStatus).

**Note:** If the data collection mode is set to `CurrentWithBackfill`, the adapter clears periods not recovered for the component and stops keeping track of them. Only if the data collection mode is set to `HistoryOnly`, an automatic history recovery operation in progress will be canceled, otherwise it will be finished.

## History recovery intervals

Automatic history intervals cannot be longer than four days. If an interval is longer than four days, the adapter automatically changes the start time of the interval to be no earlier than four days before the end time prior to starting a recovery. If a current outage lasts longer than four days, when the device status finally improves the adapter recovers up to four days before the current time. This avoids introducing additional data gaps.

## Behavior changes with client failover enabled

For adapters that support client failover, the automatic history recovery behavior is affected by whether or not you configure client failover for the running adapter instance. See [Client failover configuration](../client-failover.md) for configuring cilent failover on supported adapters. 
- When client failover is disabled, the automatic history recovery works as described in [Automatic history recovery](#automatic-history-recovery) without any behavior changes.
- When client failover is enabled, automatic history recovery is affected as listed below.
  - The tracking of **DeviceStatus**, creation of history recovery intervals, and automatic data backfill only occur when the adapter is in the `Primary` role. 
  - The adapter does not track the `Shutdown` status of the adapter to create a new history recovery interval.
  - The adapter, with its failover role changed from `Primary` to `Secondary`, stops tracking all open intervals and closes them all.
  - The adapter attempts to recover data for existing intervals only in the `Primary` role.
