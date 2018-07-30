# Form external

Forms MAY fetch external data through an HTTP GET request from a JSON REST API.

### Schemas

* [External select](#external-select-schema)
* [External data](#external-data-schema)

[JSON Schema](https://specs.livecontracts.io/v0.1.0/form-external/schema.json) | [changelog](changelog.md)

### Example

```json
{
    "$schema": "https://specs.livecontracts.io/v0.1.0/form-external/schema.json#",
    "id": "lt:/forms/6140f806-c4e6-4c22-ba3e-f0291a486cd5",
    "definition": [
        {
            "fields": [
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form-external/schema.json#external_select",
                    "label": "Post",
                    "name": "post",
                    "url": "http://jsonplaceholder.typicode.com/posts",
                    "optionValue": "id",
                    "optionText": "title"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#number",
                    "label": "Photo id",
                    "name": "photo_id",
                    "helptext": "Between 1 and 5000",
                    "conditions": "conditional_step.show_fields == 'show'",
                    "decimals": "0",
                    "min": "1",
                    "max": "5000"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form-external/schema.json#external_data",
                    "name": "photo",
                    "url": "https://jsonplaceholder.typicode.com/photos/{{ conditional_step.photo_id }}",
                    "conditions": "conditional_step.photo_id != \"\" && conditional_step.show_fields == 'show'",
                    "url_field": "conditional_step.photo-url"
                }
            ]
        }
    ]
}
```

#### Full example

[See the full example with all field types](example.md)

### External Select schema

`https://specs.livecontracts.io/v0.1.0/form-external/schema.json#external-select`

Input control to select from an external source.

### External Data schema

`https://specs.livecontracts.io/v0.1.0/form-external/schema.json#external-data`

Fetch external data. The data is fetched when initiating the form or when `url` parameter changes.

