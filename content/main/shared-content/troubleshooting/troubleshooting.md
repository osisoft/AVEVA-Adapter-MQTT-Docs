---
uid: TroubleshootTheAdapter
---

# Troubleshooting

PI adapters provide features for troubleshooting issues related to connectivity, data flow, and configuration. Resources include adapter logs and the Wireshark troubleshooting tool . If you are still unable to resolve issues or need additional guidance, contact OSIsoft Technical Support through the [OSIsoft Customer Portal](https://my.osisoft.com/).

**Note:** Make sure to also check the troubleshooting information specific to your adapter in this user guide.

## Logs

Messages from the System and OmfEgress logs provide information on the status of the adapter. For example, they show if a connection from the adapter to an egress endpoint exists.

Perform the following steps to view the System and OmfEgress logs:

1. Navigate to the logs directory:<br>
    Windows: `%ProgramData%\OSIsoft\Adapters\<AdapterName>\Logs`<br>
    Linux: `/usr/share/OSIsoft/Adapters/<AdapterName>/Logs`.<br><br>
    **Example:**<br> A successful connection to a PI Web API egress endpoint displays the following message in the OmfEgress log:

    ```json
    2020-11-02 11:08:51.870 -06:00 [Information] Data will be sent to the following OMF endpoint: 
    Id: <omfegress id>
    Endpoint: <pi web api URL>   (note: the pi web api default port is 443)
    ValidateEndpointCertificate: <true or false>
    ```

2. Optional: Change the log level of the adapter to receive more information and context. For more information, see [Logging configuration](xref:LoggingConfiguration).

### ASP .NET Core platform log

The ASP .NET Core platform log provides information from the Kestrel web server that hosts the application. The log could contain information that the adapter is overloaded with incoming data. Perform the following steps to spread the load among multiple adapters:

1. Decrease the scan frequency.
2. Lower the amount of data selection items.

## Wireshark

Wireshark is a protocol-specific troubleshooting tool that supports all current adapter protocols. Perform the following steps if you want to use Wireshark to capture traffic from the data source to the adapter or from the adapter to the OMF destination.

1. Download [Wireshark](https://www.wireshark.org/download.html).
2. Familiarize yourself with the tool and read the [Wireshark user guide](https://www.wireshark.org/docs/wsug_html_chunked/).

## Health and diagnostics egress to PI Web API

The adapter sends health and diagnostics data to PI Web API; in some cases, conflicts may occur that are due to changes or perceived changes in PI Web API. For example, a `409 - Conflict` error message displays if you upgrade your adapter version and the PI points do not match in  the upgraded version. However, data is continued to be sent as long as containers are created, so buffering only starts if no containers are created.

To resolve the conflict, perform the following steps:

1. Stop the adapter.
2. Delete the `Health` folder inside of the `Buffers` folder.
3. Stop PI Web API.
4. Delete the relevant adapter created AF structure.
5. Delete the associated health and diagnostics PI points on any or all PI Data Archives created by PI Web API.
6. Start PI Web API.
7. Start the adapter.

## Adapter connection to egress endpoint

Certain egress health information in both PI Web API and OCS show if an adapter connection to an egress endpoint exists. To verify an active connection, perform one of the following procedures:

### PI Web API connection

Perform the following steps to determine if a connection to the PI Web API endpoint exists:

1. Open PI Web API.
2. Select the OmfEgress component of your adapter, for example *GVAdapterUbuntu.\<AdapterName\>.OmfEgress*.
3. Make sure that the following PI points have been created for your egress endpoint:
    - **DeviceStatus**
    - **NextHealthMessageExpected**
    - **IORate**

### OCS connection

Perform the following steps to determine if a connection to the OCS endpoint exists:

1. Open OCS.
2. Click **Sequential Data Store** > **Streams**.
3. Makes sure that  the following streams have been created for your egress endpoint:
    - **DeviceStatus**
    - **NextHealthMessageExpected**
    - **IORate**

### TCP connection

Perform the following steps to see all established TCP sessions in Linux:

1. Open a terminal.
2. Type the following command: `ss  -o state established -t -p`
3. Press Enter.
