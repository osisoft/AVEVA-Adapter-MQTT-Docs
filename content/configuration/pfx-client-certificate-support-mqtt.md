---
uid: PFXClientCertificateMQTT
---

# PFX client certificate support 

If the MQTT server requires a customer-generated PFX client certificate for connections, place the PFX certificate in the appropriate folder on your designated drive. For Windows users, you can find the folder at *C:\ProgramData\OSIsoft\Adapters\MQTT\Certificates*. Linux users can find the folder at */usr/share/OSIsoft/Adapters/MQTT/Certificates.*

After placing the certificate in the folder, specify the PFX client certificate's thumbprint and password in the Data Source configuration of the adapter. 

For example: 

```code
{ 
    "hostNameOrIpAddress": "10.0.0.75",
    "port": 8883,
    "protocol": "Tcp",
    "TLS": "Tls13",
    "userName":null,
    "password": null,
    "clientId": "25f996f9-07e1-4912-bb9e-0d116835a9bc",
    "clientCertificateThumbprint": "440f75c98da925630a08ac5f736ced94fc141ee1",
    "clientCertificatePassword": "ThePassword",
    "MQTTVersion": "3.1.1",
    "validateServerCertificate": true,    
    "streamIdPrefix": null,
    "defaultStreamIdPattern": "{BaseTopic}.{MetricName}"
}
```


