---
uid: SparkplugBDataSourceDiscovery
---

# Data source discovery (Sparkplug B)

A discovery against the data source of an MQTT Sparkplug B adapter allows you to specify the optional **query** parameter. The query discovers the contents of the data source and narrows down the scope of the discovery. You can add the discovered items to the data selection.

## Sparkplug B query string

The string of the **query** parameter must contain string items in the following form: <br>`Topics=<Group_Id>/<Edge_Node_Id>/[<Device_Id>];WaitTime=<WaitTime>`<br><br>

| String item      | Required | Description |
|------------------|----------|-------------|
| **Group_Id**     | Optional | This element of the Topic namespace logically groups MQTT EoN nodes into the MQTT server and the consuming MQTT clients.|
| **Edge_Node_Id** | Optional | This element of the Sparkplug Topic namespace uniquely identifies the MQTT EoN node within the infrastructure.|<br>**Group_Id** combined with **Edge_Node_Id** must be unique from any other **Group_Id**/**Edge_Node_Id** assigned in the MQTT infrastructure.
| **Device_Id**    | Optional | This element of the Sparkplug Topic namespace identifies a device attached to the MQTT EoN node (physically or logically). |
| **WaitTime**     | Optional |  The time window in which the adapter will perform the discovery by listening for specified topics. Once the wait time has elapsed, the discovery results will be available.<br><br>Minimum value: `0.00:00:30` (30 seconds)<br>Maximum value: `7.00:00:00` (7 days)<br>**Note:** If you do not specify a wait time, the default value `0.00:01:00` (1 minute) will be applied.<br>When you specify **WaitTime** with a numeric value, for example `WaitTime=45`, the adapter interprets the value as seconds instead of time span.          |

### Query rules

The following rules apply for specifying the query string:

- Multiple queries are separated by a semicolon (`;`).
- Partial queries are terminated by a multi-level wildcard (`#`).
- A query cannot be terminated by a trailing slash (`/`).
- A query cannot start with a leading slash (`/`) or `$`.
- Topics are case sensitive.

**Note:** The data source might contain large amounts of metrics. Use the `#` judiciously and narrow down the query string to something specific or break down the query into different discoveries.

#### Wildcards

Wildcards are allowed in the query with the following specifications:

- A single-level wildcard replaces one topic level and is indicated by `+`.
- A multi-level wildcard covers many topic levels and is indicated by `#`.
- Wildcards can be combined.
- `#` must not be used more than once and can only be used at the end of the topic.
- No query, an empty string, or `null` as the query parameter is equivalent to `#`.

## Discovery query example

The query parameter of the MQTT Sparkplug B component must be specified in the following form:
`<Group_Id>/<Edge_Node_Id>/<[Device_Id]>`.

### Sparkplug B data source discovery initiation

```json
{
	"id" : "40",
	"query" : "topics=group1/edgeNode1,group2/edgeNode2;waitTime=00:00:30"
}
```

### Sparkplug B data source discovery results

```json
[
    {
	    "id": "40",
	    "query": "topics=group1/edgeNode1,group2/edgeNode2;waitTime=00:00:30",
	    "startTime": "2020-12-14T14:19:01.4383791-08:00",
	    "endTime": "2020-12-14T14:19:31.8549164-08:00",
	    "progress": 30,
	    "itemsFound": 700,
	    "newItems": 200,
	    "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/mqttComponentId/Discoveries/40/result",
	    "autoSelect": false,
	    "status": "Complete",
	    "errors": null
	}
]
```
