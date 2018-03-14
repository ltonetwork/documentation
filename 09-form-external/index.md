[[← back](../)]

# Form external

Forms MAY fetch external data through an HTTP GET request from a JSON REST API.

## Schemas

[External select](#external-select-schema)
[External data](#external-data-schema)

## External Select schema

[JSON Schema](schema.json#external-select)

Input control to select from an external source.

## External Data schema

[JSON Schema](schema.json#external-data)

Fetch external data. The data is fetched when initiating the form or when `url` parameter changes.

