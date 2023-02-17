---
uid: InstallTheAdapter
---

# Install the adapter

You can install adapters on either a Windows or Linux operating system. Before installing the adapter, see the respective system requirements to ensure your machine is properly configured to provide optimum adapter operation.

## Windows

Complete the following steps to install an AVEVA adapter on a Windows computer:

1. Download <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.msi</code> from the [OSIsoft Customer portal](https://customers.osisoft.com/s/products).

    **Note:** Customer login credentials are required to access the portal.

2. Run <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.msi</code> file.

3. Follow the setup wizard.

    You can change the installation folder or port number during setup. The default port number is `5590`.

4. Optional: To verify the installation, run the following `curl` command with the port number that you specified during installation:

    ```bash
   curl http://localhost:5590/api/v1/configuration
   ```

   If you receive an error, wait a few seconds and try the script again. If the installation was successful, a JSON copy of the default system configuration is returned.

## Linux

Complete the following steps to install an adapter on a Linux computer:

1. Download the appropriate Linux distribution file (<code>[!include[installer](../_includes/inline/installer-name.md)]-<var>platform</var>_.deb</code>) from the [OSIsoft Customer portal](https://customers.osisoft.com/s/products).

    **Note:** Customer login credentials are required to access the portal.

2. Open a terminal.

3. Run the `sudo apt update` command to update available packages information.

4. Run the `sudo apt install` command against the Linux distribution file (<code>[!include[installer](../_includes/inline/installer-name.md)]-<var>platform</var>_.deb</code>) selected in step 1 of this procedure.

    For example: <code>sudo apt install ./[!include[installer](../_includes/inline/installer-name.md)]-x64_.deb</code>

5. Optional: To verify the installation, run the following `curl` command with the port number that you specified during installation:

   ```bash
   curl http://localhost:5590/api/v1/configuration
   ```

    If you receive an error, wait a few seconds and run the command again. If the installation was successful, a JSON copy of the default system configuration is returned.
