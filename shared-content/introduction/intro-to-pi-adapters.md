---
uid: IntroToPiAdapters
---

# [!include[product-name](../_includes/inline/product-name.md)] [!include[product-version](../_includes/inline/product-version.md)]

[!include[product-name](../_includes/inline/product-name.md)] is a  data collection technology that collects time-series operations data from a data source over the [!include[product-protocol](../_includes/inline/product-protocol.md)] protocol and then sends it to a supported storage location in the Open Message Format (OMF).

<!-- Add content about the protocol here -->

## [!include[product-name](../_includes/inline/product-name.md)] data flow

The following diagram depicts the collection and processing of data for an operational [!include[product-name](../_includes/inline/product-name.md)], collecting and processing data. Refer to the list below the diagram for more information on each callout depicted.

<!-- Mark Bishop 3/3/22: The SVG file referenced below can be opened and edited using https://app.diagrams.net/ -->

![Adapter Data Flow](../images/adapter-data-flow.drawio.svg)

1. The user installs and configures [!include[product-name](../_includes/inline/product-name.md)] on a host system. You can configure the adapter using either a REST interface or EdgeCmd, a command line utility specifically designed for interfacing with edge systems.
   
1. The adapter collects data from assets over the [!include[product-protocol](../_includes/inline/product-protocol.md)] protocol, a process known as *data ingress*.

1. The adapter converts ingress data to the Open Message Format (OMF), a format that supported storage locations understand.

1. The adapter sends OMF data to a supported storage location in a process known as *data egress*. Supported egress endpoints include:

   * AVEVA Server
   * AVEVA Data Hub
