---
uid: ReleaseNotesMQTT
---

# AVEVA Adapter for MQTT

[!include[produce-name](../main/shared-content/_includes/inline/product-name.md)] [!include[symantic-version](../main/shared-content/_includes/inline/symantic-version.md)]<br>

Adapter framework [!include[framework-version](../main/shared-content/_includes/inline/framework-version.md)] <br>

## Overview

AVEVA Adapter for MQTT collects time-series data from MQTT servers, its service nodes/devices adhering to the SparkplugB specification, but also generic producing JSON payload. It then sends the data to configured OMF endpoints such as PI Web API and OSIsoft Cloud Services.

AVEVA Adapter for MQTT is capable of collecting health and diagnostics information. It supports buffering, unsolicited data collection, automatic discovery of available data items on a SparkplugB data source, various Windows and Linux-based operating systems, and containerization.

This version is released with the support for client failover and server failover functionality.

For more information see the [AVEVA Adapter for MQTT overview](xref:index).

## Fixes and enhancements

This updated release contains enhancements and adapter framework updates.

### Fixes

The following items were added in release 1.3.0.99:

None

### Enhancements

The following items were added in release 1.3.0.99:

| Item              | Description               |
| ----------------- | ------------------------- |
| 166669 | Server failover support for AVEVA Adapter for MQTT |
| 290389 | Client failover support for AVEVA Adapter for MQTT |

## Known issues

The following problems and enhancement requests have been deferred until a future release: 

| Item              | Description               |
| ----------------- | ------------------------- |
| 381808 | At the configuration time, if the configured Data Source has already established a data flow, then when configuring the Redundant Servers, the data flow will be interrupted for a short time period (a no data period in milliseconds or a few seconds) due to connection refresh. |
| 381814 | When performing the initial instance of Server-side failover, the secondary server becomes the target and the adapter will successfully failover back to the original server. However, subsequent attempts to perform Server-side failover results in numerous disconnects and reconnects if you do not restart the adapter. **Note:** This case is rare and restarting the adapter solves the issue. <br> <br> Additionally, we recommend restarting the adapter after every change of Client Failover mode of secondary adapter in order to avoid potential loss of data when failover occurs. |

## Setup

### System requirements

Refer to [System requirements](xref:SystemRequirements).

### Installation and upgrade

Refer to [Install the adapter](xref:InstallTheAdapter).

### Uninstallation

Refer to [Uninstall the adapter](xref:UninstallTheAdapter).

## Security information and guidance

OSIsoft is [committed to releasing secure products](https://docs.osisoft.com/bundle/security-commitment-and-disclosure-standards/page/securitycommitmentanddisclosurestandards.html). This section is intended to provide relevant security-related information to guide your installation or upgrade decision.  

OSIsoft [proactively discloses](https://docs.osisoft.com/bundle/security-commitment-and-disclosure-standards/page/securitycommitmentanddisclosurestandards.html#vulnerability-communication) aggregate information about the number and severity of security vulnerabilities addressed in each release. The tables below provide an overview of security issues addressed and their relative severity based on [standard scoring](https://docs.osisoft.com/bundle/security-commitment-and-disclosure-standards/page/securitycommitmentanddisclosurestandards.html#vulnerability-scoring). 

There are no security vulnerabilities in this release. 

