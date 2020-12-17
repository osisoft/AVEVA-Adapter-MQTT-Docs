---
uid: SparkplugBDataSourceDiscovery
---

# Sparkplug B data source discovery

A discovery against the data source of an MQTT Sparkplug B adapter allows you to specify the optional **query** parameter. The query discovers the contents of the data source and narrows down the scope of the discovery. You can add the discovered items to the data selection.

## Sparkplug B query string

The string of the **query** parameter must be contain string items in the following form: `<Group_Id>/<Edge_Node_Id>/[<Device_Id>]`

| String item      | Required | Description |
|------------------|----------|-------------|
| **Group_Id**     | Optional |
| **Edge_Node_Id** | Optional |
| **Device_Id**    | Optional |

### Query rules

The following rules apply for specifying the query string:

- Multiple queries are separated by a semicolon (`;`)
- A query cannot be terminated by a trailing slash (`/`)
- A query cannot start with a leading slash (`/`) or `$`
- Topics are case sensitive

**Note:** The data source might contain tens of thousands of metrics. Use the `#` judiciously and narrow down the query string to something specific or break down the query into different discoveries.

#### Wildcards

Wildcards are allowed in the query.

- A single-level wildcard replaces one topic level and is indicated by `+`.
- A multi-level wildcard covers many topic levels and is indicated by `#`.
- Wildcards can be combined.
- `#` must not be used more than once and only at the end of the topic.
- No query, an empty string, or `null` as the query parameter is equivalent to `#`.
- Partial queries are terminated by a multi-level wildcard.

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
        "startTime": "2020-09-30T19:34:01.8180401+02:00",
        "endTime": "2020-09-30T19:34:01.8368776+02:00",
        "progress": 0,
        "itemsFound": 0,
        "newItems": 0,
        "newAssets": 0,
        "resultUri": null,
        "autoSelect": false,
        "status": "Active",
        "errors": null
    }
]
```
