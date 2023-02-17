---
uid: UninstallTheAdapter
---

# Uninstall the adapter

Complete the procedure corresponding to your specific operating system to uninstall the adapter:

## Windows

1. To delete the AVEVA adapter program files from a Windows device, use the Windows Control Panel uninstall application process.

    **Note:** The configuration, data, and log files are not deleted by the uninstall process.

2. Optional: To delete data, configuration, and log files, delete the directory:

    _%ProgramData%\OSIsoft\Adapters\\[!include[product-name](../_includes/inline/component-type.md)]_
   
   This deletes all data processed by the adapter, in addition to the configuration and log files.

## Linux

1. To delete AVEVA Adapter software from a Linux device, open a terminal window and run the following command:

    <!-- PRERELEASE REMINDER: Customize for {adapter-name}. Example:BACnet, EventHubs, StructuredDataFiles, etc -->

    ```bash
    sudo apt remove aveva.adapter.{adapter-name} 
    ```

2. Optional: To delete data, configuration, and log files, run the following command:

    <!-- PRERELEASE REMINDER: Customize for {adapter-name}. Example:BACnet, EventHubs, StructuredDataFiles, etc -->

    ```bash
    sudo rm -r /usr/share/OSIsoft/Adapters/{adapter-name}
    ```
    
    This deletes all data processed by the adapter, in addition to the configuration and log files.
