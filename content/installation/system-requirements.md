---
uid: MQTTSystemRequirements
---

# System requirements

AVEVA Adapter for MQTT is supported on a variety of platforms and processors. Install kits are available for the following platforms:

| Operating System | Platform | Installation Kit | Processor(s) |
|-------------------|-------------|----------------------------------|-------------|
| Windows 10 Enterprise <br> Windows 10 IoT Enterprise <br> Windows 11 Enterprise <br> Windows Server 2019 <br> Windows Server 2022 | x64 | <code>AVEVA-Adapter-for-MQTT_1.3.0.103-x64_.msi</code>     | Intel/AMD 64-bit processors |
| Debian 10, 11 <br> Ubuntu 20.04, 22.04 | x64 | <code>AVEVA-Adapter-for-MQTT_1.3.0.103-x64_.deb</code>     | Intel/AMD 64-bit processors |
| Debian 10, 11 <br> Ubuntu 20.04, 22.04 | ARM32 | <code>AVEVA-Adapter-for-MQTT_1.3.0.103-arm_.deb</code>  | ARM 32-bit processors |
| Debian 10, 11 <br> Ubuntu 20.04, 22.04 | ARM64 | <code>AVEVA-Adapter-for-MQTT_1.3.0.103-arm64_.deb</code>  | ARM 64-bit processors |

Alternatively, you can use tar.gz files with binaries to build your own custom installers or containers for Linux. For more information on installing the adapter with Docker containers, see <xref:InstallUsingDocker>.

## PI Web API compatibility

This version of AVEVA Adapter for MQTT is compatible with PI Web API 2021 and later. 

## Performance Metrics

A wide range of Linux and Windows based devices support AVEVA Adapters, from small single board computers to large servers. Stream count and event throughput vary based on the device's hardware specification. 

Performance guidelines for AVEVA Adapters when running on typical small and large devices are as follows:

- Small Devices - 1 Core CPU, 512 MB RAM. 1k events/sec, 1k streams total
- Large Devices - 2 Core CPU, 8 GB RAM. 20k events/sec, 20k streams total

Actual performance of AVEVA Adapters vary depending on different factors, such as the performance of data source, network latency, and hardware resource consumption by other applications on the device. We recommend solid state storage on the device to improve the performance, robustness, and reliability of the AVEVA Adapter data buffering mechanism. 
