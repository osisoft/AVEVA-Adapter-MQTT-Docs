---
uid: PIAdapterForMQTTPrinciplesOfOperation
---

# PI Adapter for MQTT principles of operation

This adapter's operations focus on data collection and stream creation.

## Adapter configuration

For the MQTT adapter to start data collection, configure the following:

- Data source: Provide the data source from which the adapter should collect data.<br> For more details, see the data source topics [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration) and [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).
- Data selection: Select MQTT items to which the adapter should subscribe for data. <br> For more details, see the data selection topics [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration) and [PI Adapter for MQTT Sparkplug B data selection configuration](xref:PIAdapterForMQTTSparkplugB).
- Logging: Set up the logging attributes to manage the adapter logging behavior.

## Connection

The adapter uses either the Transmission Control Protocol (TCP) or WebSocket protocol (WS) to speak to an MQTT server. For more information on how to configure which protocol should be used, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration) and [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).

## Data collection

<!--
Comments from Bo:
We may want to explicitly emphysize that the genric MQTT adapter only supports JSON payload. 
The statement below may not be that obvious and may not bring enough attention to the user.
-->
<!--TU: Rewrote section-->
The adapter collects time-series data from the MQTT server using Topics. The generic MQTT adapter only supports data from devices producing Json payload, the MQTT Sparkplug B adapter also supports data from devices adhering to the Sparkplug B specification. For more information see [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration) and [PI Adapter for MQTT Sparkplug B data selection configuration](xref:PIAdapterForMQTTSparkplugB).

### Data types
<!--
Comments from Bo: 
The data types section only lists the supported data types for generic MQTT adapter, but not MQTT Sparkplug B. There are some overlaps regarding supported data types, but there are also adapter-specific data types supported by each component. 
Thyag can provide information about the supported data types for Sparkplug B component. With that being said, the structure of this section may need to be adjusted to have two sub-section for two components.
-->
<!--
Thyag
For SpB the data types are defined in https://www.eclipse.org/tahu/spec/Sparkplug%20Topic%20Namespace%20and%20State%20ManagementV2.2-with%20appendix%20B%20format%20-%20Eclipse.pdf Sec. 15.2.1. Metric Datatypes
We might consider having a separate table as suggested by Bo.
-->
The following table lists MQTT variable types that the adapter collects data from and types of streams that will be created.

| MQTT data type | Stream data type |
|------------------|------------------|
| Byte             | Int16            |
| SByte            | Int16            |
| Int16            | Int16            |
| Int32            | Int32            |
| Int64            | Int64            |
| UInt16           | UInt16           |
| UInt32           | UInt32           |
| UInt64           | UInt64           |
| Float            | Float32          |
| Double           | Float64          |
| Boolean          | Boolean          |
| String           | String           |
| DateTime         | DateTime         |

<!-- TU: Starting a new section--->

#### MQTT Sparkplug B data types

The following table lists MQTT Sparkplug B variable types that the adapter collects data from and types of streams that will be created.

| MQTT Sparkplug B data type | Stream data type |
|----------------------------|------------------|
| Int8                       | Int8             |
| Int16                      | Int16            |
| Int32                      | Int32            |
| Int64                      | Int64            |
| UInt8                      | UInt8            |
| UInt16                     | UInt16           |
| UInt32                     | UInt32           |
| UInt64                     | UInt64           |
| Float                      | Float32          |
| Double                     | Float64          |
| Boolean                    | Boolean          |
| String                     | String           |
| DateTime                   | UInt64           |
| Text                       | String           |

## Stream creation

The MQTT adapter creates a stream with two properties for each selected MQTT item. The properties are described in the following table.

| Property name | Data type | Description |
|---------------|-----------|-------------|
| `Timestamp`   | String    | The response time of the stream data from the MQTT device |
| `Value`       | Specified by the data selection | The value of the stream data from the MQTT device |

Certain metadata are sent with each stream created. The following metadata are common for every adapter type:

- **ComponentId**: Specifies the data source, for example, _MQTT1_
- **ComponentType**: Specifies the type of adapter, for example, _MQTT_

For MQTT specific metadata, see [MQTT specific metadata](xref:MQTTSpecificMetadata1-3) and for MQTT Sparkplug B specific metadata, see [MQTT Sparkplug B specific metadata](xref:MQTTSparkplugBSpecificMetadata1-3).