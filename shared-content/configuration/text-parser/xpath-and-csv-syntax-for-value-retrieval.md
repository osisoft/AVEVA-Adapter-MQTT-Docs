---
uid: XPathAndCSVSyntaxForValueRetrieval
---

# XPath and CSV syntax for value retrieval

For information on which semantics are used for retrieving values from XML and CSV files, see the following documentation:

- XML - [XML Path Language (XPath)](https://www.w3.org/TR/1999/REC-xpath-19991116/)
- CSV - Column Index (1 based) or Header value (if header defined)

The following syntaxes are used to extract values from XML or CSV documents.

### XML - Simple XPath example

```xml
<values>
  <value>
    <time>2020-08-10T12:10:46.0928791Z</time>
    <value>1.234567890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:47.0928791Z</time>
    <value>12.34567890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:48.0928791Z</time>
    <value>123.4567890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:49.0928791Z</time>
    <value>1234.567890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:50.0928791Z</time>
    <value>12345.67890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:51.0928791Z</time>
    <value>123456.7890</value>
  </value>
  <value>
    <time>2020-08-10T12:10:52.0928791Z</time>
    <value>12345678.90</value>
  </value>
  <value>
    <time>2020-08-10T12:10:53.0928791Z</time>
    <value>123456789.0</value>
  </value>
</values>
```

The following XPath configuration reads a series of values:

```json
{
    "Id": "DoubleValue",
    "FieldDefinition": "./values/value/value",
    "DataType": "Double"
},
{
     "Id": "Timestamp",
    "FieldDefinition": "./values/value/time",
    "DataType": "DateTime",
    "IsIndex": true
}
```

### CSV - Simple CSV column index example

```csv
2020-08-10T12:10:46.0928791Z,1.234567890
2020-08-10T12:10:47.0928791Z,12.34567890
2020-08-10T12:10:48.0928791Z,123.4567890
2020-08-10T12:10:49.0928791Z,1234.567890
2020-08-10T12:10:50.0928791Z,12345.67890
2020-08-10T12:10:51.0928791Z,123456.7890
2020-08-10T12:10:52.0928791Z,12345678.90
2020-08-10T12:10:53.0928791Z,123456789.0
```

The following CSV column index configuration requires the text parser be configured with `HasHeader=false`. The column indexes are 1 based and configured as strings.

```json
{
    "Id": "DoubleValue",
    "FieldDefinition": "2",
    "DataType": "Double"
},
{
    "Id": "Timestamp",
    "FieldDefinition": "1",
    "DataType": "DateTime",
    "IsIndex": true
}
```

### CSV - Simple CSV column header example

```csv
Date,Value
2020-08-10T12:10:46.0928791Z,1.234567890
2020-08-10T12:10:47.0928791Z,12.34567890
2020-08-10T12:10:48.0928791Z,123.4567890
2020-08-10T12:10:49.0928791Z,1234.567890
2020-08-10T12:10:50.0928791Z,12345.67890
2020-08-10T12:10:51.0928791Z,123456.7890
2020-08-10T12:10:52.0928791Z,12345678.90
2020-08-10T12:10:53.0928791Z,123456789.0
```

The following CSV column header configuration requires the text parser be configured with `HasHeader=true`.

```json
{
    "Id": "DoubleValue",
    "FieldDefinition": "Value",
    "DataType": "Double"
},
{
    "Id": "Timestamp",
    "FieldDefinition": "Date",
    "DataType": "DateTime",
    "IsIndex": true
}
```
