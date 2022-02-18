---
uid: UpgradeTheAdapter
---

# Upgrade the adapter

When a new version of the adapter is released, you can upgrade to the latest version by running the new installation package.

## Windows upgrade

Complete the following steps to upgrade a PI adapter on a Windows computer:

1. Download <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.msi</code> from the [OSIsoft Customer Portal](https://customers.osisoft.com/s/products).

    **Note:** Customer login credentials are required to access the portal.

2. Run <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.msi</code>.

3. Complete the setup wizard.

4. Optional: To verify the upgrade, run the following `curl` command with the port number that you specified when completing the wizard:

   ```powershell
   curl -X GET "http://localhost:5590/api/v1/Diagnostics/ProductInformation"
   ```

   Upon successful upgrade, the JSON response lists the updated application version:

   ```json
   {
    "Application Version": "1.1.0.128",                 // upgraded version
    ".Net Core Version": ".NET Core 3.1.15",
    "Operating System": "Microsoft Windows 10.0.19041"
   }
   ```

## Linux upgrade

Complete the following steps to upgrade a PI adapter on a Linux computer:

1. Download the appropriate Linux distribution file from the [OSIsoft Customer Portal](https://customers.osisoft.com/s/products).

    **Note:** Customer login credentials are required to access the portal.

2. Open a terminal session.

3. Move the Linux distribution file to the target host and run the `sudo apt upgrade` command.

    Platform | Command
    --|--
    **Linux x64** |<code>sudo apt upgrade ./[!include[installer](../_includes/inline/installer-name.md)]-x64_.deb</code> 
    **Linux ARM32 Debian** |<code>sudo apt upgrade ./[!include[installer](../_includes/inline/installer-name.md)]-arm_.deb</code> 
    **Linux ARM64 Debian** |<code>sudo apt upgrade ./[!include[installer](../_includes/inline/installer-name.md)]-arm64_.deb</code> 

4. Optional: To verify the upgrade, run the following `curl` command with the port number that you specified:

   ```bash
   curl -X GET "http://localhost:5590/api/v1/Diagnostics/ProductInformation"
   ```

   Upon successful upgrade, the JSON response lists the updated application version:

   ```json
   {
    "Application Version": "1.1.0.128",                 // upgraded version
    ".Net Core Version": ".NET Core 3.1.15",
    "Operating System": "Microsoft Windows 10.0.19041"
   }
   ```
