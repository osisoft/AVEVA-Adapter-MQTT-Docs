---
uid: StartAndStopAnAdapter
---

# Start and stop an adapter

Complete the procedure appropriate for your operating system to start or stop an adapter service:

## Windows

1. Open Windows services.

2. Select **PI Adapter for \<AdapterName\>**.

3. Depending on whether your adapter is running or not, click either **Start** or **Stop**.

## Linux

1. Open command line.

2. Depending on whether your adapter is running or not, type one of the following commands:

    Example:

    _Start_ PI Adapter for \<AdapterName\>

    ```cmdline
    sudo systemctl start pi.adapter.<adapterName>
    ```

    Example:

    _Stop_ PI Adapter for \<AdapterName\>
  
      ```cmdline
      sudo systemctl stop pi.adapter.<adapterName>
      ```
  
3. Press Enter.
