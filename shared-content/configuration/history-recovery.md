---
uid: HistoryRecovery
---

# History recovery

The adapter you are using supports the following data collection modes which you configure in the **DataCollectionMode** parameter of your adapter's data source configuration:

- `CurrentOnly`: The adapter component operates normally. History recovery is disabled.
- `CurrentWithBackfill` (Default): The adapter component operates normally, but disconnections and shutdown events are recorded in the form of recovery intervals. When the adapter is reconnected to a data source, it automatically backfills data for the recorded intervals.
- `HistoryOnly`: The adapter component does not get started. The adapter is able to start collecting historical data on demand.

History recovery for adapters supports the following two operations related to the data collection mode:

- **On demand history recovery**: Recovers data from a specified start time or start and end time. If end time is not specified, the default is `utcnow`. On demand history recovery is available only when the adapter is in `HistoryOnly` data collection mode.

- **Limited automatic history recovery**: Backfills data gaps that originated from connection disruptions, data source issues, or AVEVA adapter shutdown or both. This is limited to a maximum time-range of four days. Limited automatic history recovery is available only when the adapter is in `CurrentWithBackfill` data collection mode.
