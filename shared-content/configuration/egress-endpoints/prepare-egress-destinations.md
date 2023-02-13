---
uid: PrepareEgressDestinations
---

# Prepare egress destinations

AVEVA Data Hub and AVEVA Server destinations may require additional configuration to receive OMF messages.

## AVEVA Data Hub

To prepare OCS to receive OMF messages from the adapter, create an OMF connection in AVEVA Data Hub. Creating an OMF connection results in an available OMF endpoint that can be used by the adapter egress mechanism. Complete the following steps to create an OMF connection:

1. Create a **Client**.

   The *Client Id* and *Client Secret* will be used for the corresponding properties in the egress configuration.

2. Create an **OMF** type **Connection**.

   The connection should link the created client to an existing [namespace](https://docs.osisoft.com/bundle/ocs/page/set-up/namespaces/namespaces-concept.html) where the data will be stored.

   The **OMF Endpoint** URL for the connection will be used as the egress configuration *Endpoint* property.

## AVEVA Server

To prepare a AVEVA Server to receive OMF messages from the adapter, a PI Web API OMF endpoint must be available. Complete the following steps:

1. Install PI Web API and enable the **Open Message Format (OMF) Services** feature.

    - During configuration, choose an AF database and PI Data Archive where metadata and data will be stored.

    - The account used in an egress configuration needs permissions to create AF elements, element templates, and PI points.

2. Configure PI Web API to use *Basic* and/or *Kerberos* authentication.

 For complete steps, as well as best practices and recommendations, see the following topic in the PI Web API User Guide: [Authentication methods](https://docs.osisoft.com/bundle/pi-web-api/page/authentication-methods.html).

**Notes:**

- The certificate used by PI Web API must be trusted by the device running the adapter, otherwise the egress configuration *ValidateEndpointCertificate* property needs to be set to false (this can be the case with a **self-signed certificate** but should only be used for testing purposes).

- To continue to send OMF egress messages to the PI Web API endpoint after upgrading PI Web API, restart the adapter service.
