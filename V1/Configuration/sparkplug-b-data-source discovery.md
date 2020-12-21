---
uid: SparkplugBDataSourceDiscovery
---

# Sparkplug B data source discovery

A discovery against the data source of an MQTT Sparkplug B adapter allows you to specify the optional **query** parameter. The query discovers the contents of the data source and narrows down the scope of the discovery. You can add the discovered items to the data selection.

## Sparkplug B query string

The string of the **query** parameter must be contain string items in the following form: <br>`<Group_Id>/<Edge_Node_Id>/[<Device_Id>]`

| String item      | Required | Description |
|------------------|----------|-------------|
| **Group_Id**     | Optional | This element of the Topic namespace logically groups MQTT EoN nodes into the MQTT server and back to the consuming MQTT clients.
| **Edge_Node_Id** | Optional | This element of the Sparkplug Topic namespace uniquely identifies the MQTT EoN node within the infrastructure.<br>**Group_Id** combined with **Edge_Node_Id** must be unique from any other ****Group_Id**/**Edge_Node_Id** assigned in the MQTT infrastructure.
| **Device_Id**    | Optional | This element of the Sparkplug Topic namespace identifies a device attached to the MQTT EoN node (physically or logically).

### Query rules

The following rules apply for specifying the query string:

- Multiple queries are separated by a semicolon (`;`).
- Partial queries are terminated by a multi-level wildcard.
- A query cannot be terminated by a trailing slash (`/`).
- A query cannot start with a leading slash (`/`) or `$`.
- Topics are case sensitive.

**Note:** The data source might contain tens of thousands of metrics. Use the `#` judiciously and narrow down the query string to something specific or break down the query into different discoveries.

#### Wildcards

Wildcards are allowed in the query.

- A single-level wildcard replaces one topic level and is indicated by `+`.
- A multi-level wildcard covers many topic levels and is indicated by `#`.
- Wildcards can be combined.
- `#` must not be used more than once and only at the end of the topic.
- No query, an empty string, or `null` as the query parameter is equivalent to `#`.

## Discovery query example

The query parameter of the MQTT Sparkplug B component must be specified in the following form:
`<Group_Id>/<Edge_Node_Id>/<[Device_Id]>`.

### Sparkplug B data source discovery initiation

```json
{
"id" : "40",
"query" : "group1/edge2"
}
```

### Sparkplug B data source discovery results

```json
[
    {
	    "id": "40",
	    "query": "group1/edge2",
	    "startTime": "2020-12-14T14:19:01.4383791-08:00",
	    "endTime": "2020-12-14T14:19:31.8549164-08:00",
	    "progress": 729,
	    "itemsFound": 729,
	    "newItems": 729,
	    "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/mqttComponentId/Discoveries/40/result",
	    "autoSelect": false,
	    "status": "Complete",
	    "errors": null
	}
]
```
