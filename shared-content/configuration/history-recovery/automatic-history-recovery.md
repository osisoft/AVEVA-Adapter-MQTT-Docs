---
uid: AutomaticHistoryRecovery
---

# Automatic history recovery

Besides on-demand history recovery, the PI adapter also supports automatic history recovery.

For automatic history recovery, the adapter tracks changes to the **DeviceStatus** of each component. When the **DeviceStatus** changes to `DeviceInError` or `Shutdown`, the adapter starts a new [History recovery interval](#history-recovery-intervals). When the issue resolves or if the adapter is restarted and the **DeviceStatus** changes to `Good`, the adapter closes any current intervals for that component. The adapter tracks these intervals for each component and, when DeviceStatus has a value of `Good`, it performs history recovery for these intervals starting from oldest to newest. For more information, see also [Device status](xref:DeviceStatus).

**Note:** If the data collection mode is set to `CurrentWithBackfill`, the adapter clears periods not recovered for the component and stops keeping track of them. Only if the data collection mode is set to `HistoryOnly`, an automatic history recovery operation in progress will be canceled, otherwise it will be finished.

## History recovery intervals

Automatic history intervals cannot be longer than four days. If an interval is longer than four days, the adapter automatically changes the start time of the interval to be no earlier than four days before the end time prior to starting a recovery. If a current outage lasts longer than four days, when the device status finally improves the adapter recovers up to four days before the current time. This avoids introducing additional data gaps.
