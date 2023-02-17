---
uid: PIAdapterForMQTTSecurity
---

# Security 

When determining MQTT security practices with regards to REST APIs, you should consider the following practice. To keep the adapter secure, only administrators should have access to machines where the adapter is installed. REST APIs are bound to localhost, meaning that only requests coming from within the machine will be accepted. 

**Note**: If the adapter requires the use of self-signed certificates, we recommend that you do not use them in production environments. When using a self-signed certificate, the certificate must be trusted by the device running the adapter.  
