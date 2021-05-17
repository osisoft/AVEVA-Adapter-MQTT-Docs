---
uid: MQTTSpecificMetadata
--- 

# MQTT specific metadata

Metadata specific to the MQTT generic component are:

- **Topic**: Specifies the MQTT topic string
- **ValueField**: Specifies the JsonPath expression used to extract the data value from a property within the payload supplied by the MQTT server

**Note:** A configured metadata level allows you to set the amount of metadata for the adapter. Specify the metadata level in [General configuration](xref:GeneralConfiguration). For the MQTT adapter, the following metadata is sent for the individual level:

<!--
Comments from Bo:
The following statements are only true for the genric MQTT component. Sparkplug B sends a different set of stream metadata. Simmlar to above, Thyag can help with the Sparkplug B specific topics. Also, we probably have to have two sub-sections to discuss the different behavior in metadata sending by two diffferent components.
-->
- `None`: No metadata
- `Low`: AdapterType (ComponentType) and DataSource (ComponentId)
- `Medium`: AdapterType (ComponentType), DataSource (ComponentId), Topic, and ValueField

Each stream created for the selected measurement has a unique identifier (stream Id). If you specify a custom stream Id for the measurement in the data selection configuration, the adapter uses that stream Id to create the stream. Otherwise, the adapter constructs the stream Id using the following format:
<!--
Comments from Bo:
The default stream Id patter listed below is true for the generic MQTT adapter but not the Sparkplug B component
The Default stream Id pattern for generic MQTT is:
<AdapterComponentId>.<Topic>.<ValueField>
The default stream Id pattern for MQTT Sparkplug B is:
<AdapterComponentId>.<Topic>.<MetricName>
Similarly, we may want to have two sub-sections to discuss all the difference including the items mentioned above
-->
```code
<AdapterComponentId>.<Topic>.<ValueField>
```

**Note:** Naming convention is affected by `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration. For more information, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration).