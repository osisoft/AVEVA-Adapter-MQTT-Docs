---
uid: UninstallTheAdapter
---

# Uninstall the adapter

Complete the procedure corresponding to your specific operating system to uninstall the adapter:

## Windows

1. To delete the PI adapter program files from a Windows device, use the Windows Control Panel uninstall application process.

    **Note:** The configuration, data, and log files are not deleted by the uninstall process.

2. Optional: To delete data, configuration, and log files, delete the directory _%ProgramData%\OSIsoft\Adapters\AdapterName_.

    This deletes all data processed by the adapter, in addition to the configuration and log files.

## Linux

1. To delete PI Adapter software from a Linux device, open a terminal window and run the following command:

    ```bash
    sudo apt remove pi.adapter.<AdapterName>
    ```

2. Optional: To delete data, configuration, and log files, run the following command:

    ```bash
    sudo rm -r /usr/share/OSIsoft/Adapters/<AdapterName>
    ```
    
    This deletes all data processed by the adapter, in addition to the configuration and log files.
