---
uid: StartAndStopIngressComponent
---

# Start and stop ingress component

To control data ingress, you can start and stop the ingress components of an adapter whenever necessary. By default, all currently configured ingress components are started.

## Start an ingress component

Complete the following steps to start an individual ingress component:

1. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests.
2. Run a `POST` command to the following endpoint, replacing `<ComponentId>` with the ingress component that you want to start: `http://localhost:5590/api/v1/administration/<ComponentId>/Start`

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Start the adapter ingress component**

    ```bash
    curl -d "" -X POST "http://localhost:5590/api/v1/Administration/<ComponentId>/Start"
    ```

## Stop an ingress component

Complete the following steps to stop an individual ingress component:

1. Start any configuration tool capable of making HTTP requests.

2. Run a `POST` command to the following endpoint, replacing `<ComponentId>` with the ingress component that you want to stop: `http://localhost:5590/api/v1/administration/<ComponentId>/Stop`

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Stop the adapter ingress component**

    ```bash
    curl -d "" -X POST "http://localhost:5590/api/v1/Administration/<ComponentId>/Stop"
    ```
