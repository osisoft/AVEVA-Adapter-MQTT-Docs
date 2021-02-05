---
uid: MQTTSparkplugBSpecificMetadata1-3
---

# MQTT Sparkplug B specific metadata

Metadata specific to the MQTT Sparkplug B component are:

- **Namespace**: Specifies the Topic namespace.
- **Group_Id**: Specifies the element of the topic namespace that logically groups MQTT EoN nodes into the MQTT server and back to the consuming MQTT clients
- **Edge_Node_Id**: Specifies the element of the topic namespace that uniquely identifies the MQTT EoN node within the infrastructure
- **Device_Id**: Specifies the element of the topic namespace that identifies a device attached to the MQTT EoN node

**Note:** A configured metadata level allows you to set the amount of metadata for the adapter. Specify the metadata level in [General configuration](xref:GeneralConfiguration). For the MQTT adapter, the following metadata is sent for the individual level:

- `None`: No metadata
- `Low`: AdapterType (ComponentType) and DataSource (ComponentId)
- `Medium`: AdapterType (ComponentType), DataSource (ComponentId), Topic, and MetricName

Each stream created for the selected measurement has a unique identifier (stream ID). If you specify a custom stream ID for the measurement in the data selection configuration, the adapter uses that stream ID to create the stream. Otherwise, the adapter constructs the stream ID using the following format:

```code
<AdapterComponentID>.<Topic>.<MetricName>
```

**Note:** Naming convention is affected by `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration. For more information, see [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).