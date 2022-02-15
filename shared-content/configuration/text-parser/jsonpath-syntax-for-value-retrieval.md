---
uid: JSONPathSyntaxForValueRetrieval
---

# JSONPath syntax for value retrieval

For information on which semantic is used for retrieving values from JSON files, see [JSONPath Syntax](https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html).

The following syntax is used to extract values from JSON documents.

## JSON - Simple JSONPath example

```json
[
  {
    "time": "2020-08-10T12:10:46.0928791Z",
    "value": 1.234567890
  },
  {
    "time": "2020-08-10T12:10:47.0928791Z",
    "value": 12.34567890
  },
  {
    "time": "2020-08-10T12:10:48.0928791Z",
    "value": 123.4567890
  },
  {
    "time": "2020-08-10T12:10:49.0928791Z",
    "value": 1234.567890
  },
  {
    "time": "2020-08-10T12:10:50.0928791Z",
    "value": 12345.67890
  },
  {
    "time": "2020-08-10T12:10:51.0928791Z",
    "value": 123456.7890
  },
  {
    "time": "2020-08-10T12:10:52.0928791Z",
    "value": 12345678.90
  },
  {
    "time": "2020-08-10T12:10:53.0928791Z",
    "value": 123456789.0
  }
]
```

The following JSONPath configuration reads a series of values:

```json
{
    "Id": "DoubleValue",
    "FieldDefinition": "value",
    "DataType": "Double"
},
{
    "Id": "Timestamp",
    "FieldDefinition": "time",
    "DataType": "DateTime",
    "IsIndex": true
}
```

## JSON - Complex JSONPath examples

The following example reads specific values from a JSON array:

```json
{
  "StreamData": {
    "TPPrototype.uflsample.value_time": [
      {
        "StreamId": "TPPrototype.uflsample.value_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T06:00:00Z",
        "Value": 339.0
      },
      {
        "StreamId": "TPPrototype.uflsample.value_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T07:00:00Z",
        "Value": 344.0
      },
      {
        "StreamId": "TPPrototype.uflsample.value_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T17:00:00Z",
        "Value": 341.0
      }
    ],
    "TPPrototype.uflsample.value_timeString": [
      {
        "StreamId": "TPPrototype.uflsample.value_timeString",
        "DataType": "String",
        "Timestamp": "2013-12-01T06:00:00Z",
        "Value": "339.0"
      },
      {
        "StreamId": "TPPrototype.uflsample.value_timeString",
        "DataType": "String",
        "Timestamp": "2013-12-01T07:00:00Z",
        "Value": "344.0"
      },
      {
        "StreamId": "TPPrototype.uflsample.value_timeString",
        "DataType": "String",
        "Timestamp": "2013-12-01T17:00:00Z",
        "Value": "341.0"
      }
    ],
    "TPPrototype.uflsample.pressure_time": [
      {
        "StreamId": "TPPrototype.uflsample.pressure_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T06:00:00Z",
        "Value": 339.0
      },
      {
        "StreamId": "TPPrototype.uflsample.pressure_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T07:00:00Z",
        "Value": 344.0
      },
      {
        "StreamId": "TPPrototype.uflsample.pressure_time",
        "DataType": "Double",
        "Timestamp": "2013-12-01T17:00:00Z",
        "Value": 341.0
      }
    ]
  }
}
```

The following JSONPath configuration reads all the `TPPrototype.uflsample.value_time` values from the JSON above:

```json
{
    "Id": "Value",
    "DataType": "Double",
    "FieldDefinition": "$['StreamData'].['TPPrototype.uflsample.value_time'][*].Value"
},
{
    "Id": "Time",
    "DataType": "DateTime",
    "FieldDefinition": "$['StreamData'].['TPPrototype.uflsample.value_time'][*].Timestamp",
    "IsIndex": true
}
```

The following example reads specific value from complex nested JSON:

```json
{
    "success": true,
    "error": null,
    "result": {
        "type": "runtime_history",
        "chart": {
            "chart": {
                "type": "column"
            },
            "title": {
                "text": ""
            },
            "subtitle": {
                "text": "Daily History"
            },
            "colors": [
                "#fee292",
                "#fdc152",
                "#f69638",
                "#f17130",
                "#9f2d26",
                "#8acadc",
                "#184c8e"
            ],
            "series": [
                {
                    "name": "Stage 3 Aux Heat",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat_aux_stage3"
                },
                {
                    "name": "Stage 2 Aux Heat",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat_aux_stage2"
                },
                {
                    "name": "Aux Heat",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat_aux"
                },
                {
                    "name": "Stage 2 Heat",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat_stage2"
                },
                {
                    "name": "Heat",
                    "data": [
                        0.0,
                        0.2,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.3,
                        0.2,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat"
                },
                {
                    "name": "Stage 2 Cool",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "cool",
                    "state": "cool_stage2"
                },
                {
                    "name": "Cool",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "cool",
                    "state": "cool"
                }
            ],
            "xAxis": {
                "categories": [
                    "Friday",
                    "Saturday",
                    "Sunday",
                    "Monday",
                    "Tuesday",
                    "Wednesday",
                    "Thursday",
                    "Friday",
                    "Saturday"
                ],
                "labels": {
                    "rotation": -45
                }
            },
            "yAxis": {
                "allowDecimals": false,
                "min": 0,
                "max": 24,
                "tickInternval": 4,
                "title": {
                    "text": "Runtime (Hours)"
                }
            },
            "legend": {
                "layout": "vertical",
                "align": "center",
                "floating": false,
                "shadow": false,
                "itemStyle": {
                    "fontSize": "1em"
                }
            },
            "tooltip": {
                "shared": true,
                "borderColor": "#000000"
            },
            "credits": {
                "enabled": false
            },
            "plotOptions": {
                "column": {
                    "stacking": "normal"
                },
                "series": {
                    "shadow": false
                }
            }
        },
        "table": {
            "headings": [
                "Fri",
                "Sat",
                "Sun",
                "Mon",
                "Tue",
                "Wed",
                "Thu",
                "Fri",
                "Sat"
            ],
            "series": [
                {
                    "name": "Aux Heat",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": "heat",
                    "state": "heat_aux"
                },
                {
                    "name": "Outdoor High Temp.",
                    "data": [
                        72.0,
                        64.0,
                        73.0,
                        72.0,
                        67.0,
                        73.0,
                        77.0,
                        62.0,
                        51.0
                    ],
                    "stack": null,
                    "state": "outdoor_high_temperature"
                },
                {
                    "name": "Outdoor Low Temp.",
                    "data": [
                        55.0,
                        60.0,
                        62.0,
                        61.0,
                        51.0,
                        43.0,
                        46.0,
                        44.0,
                        35.0
                    ],
                    "stack": null,
                    "state": "outdoor_low_temperature"
                },
                {
                    "name": "Avg Indoor Temp.",
                    "data": [
                        76.0,
                        77.0,
                        78.0,
                        78.0,
                        77.0,
                        73.0,
                        74.0,
                        75.0,
                        72.0
                    ],
                    "stack": null,
                    "state": "average_indoor_temperature"
                },
                {
                    "name": "Avg Indoor Humidity",
                    "data": [
                        66.0,
                        68.0,
                        70.0,
                        70.0,
                        69.0,
                        67.0,
                        67.0,
                        66.0,
                        61.0
                    ],
                    "stack": null,
                    "state": "average_indoor_humidity"
                },
                {
                    "name": "Fan Only Runtime",
                    "data": [
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0,
                        0.0
                    ],
                    "stack": null,
                    "state": "fan_only"
                },
                {
                    "name": "Vent",
                    "data": [],
                    "stack": null,
                    "state": "vent"
                }
            ]
        },
        "show_monthly_runtime_history": true
    }
}
```

The following JSONPath configuration reads Sunday Average Indoor Temperature. The timestamp comes from Adapter local time.

```json
{
    "Id": "Temperature",
    "DataType": "Double",
    "FieldDefinition": "$.result.table.series[3].data[2]"
},
{
    "Id": "Timestamp",
    "DataType": "DateTime",
    "Format": "Adapter",
    "IsIndex": true
}
```

## Error handling

If you encounter text parser related errors that is errors for the **ValueField** or **IndexField**, check the StreamId associated with the error message.

Possible errors include the following:

- The JSONPath expression of **ValueField** or **IndexField** is pointing to a non-existing value
- The JSONPath expression of **ValueField** or **IndexField** is missing a value altogether
- **DataType** does not match the value