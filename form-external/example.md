## Form external example

```json
{
    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#",
    "id": "lt:/forms/dcf13bda-a432-44fb-bdc3-52e37e809018",
    "title": "Demo form for external data",
    "definition": [
        {
            "label": "External",
            "group": "external",
            "helptext": "<p>This step will perform an extern request at <a href=\"https://jsonplaceholder.typicode.com\">https://jsonplaceholder.typicode.com</a>.</p><p>Try typing in <code>est</code>.</p>",
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
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#external_select",
                    "label": "Comment",
                    "name": "comment",
                    "url": "http://jsonplaceholder.typicode.com/posts/{{ external.post }}/comments",
                    "optionValue": "id",
                    "optionText": "name"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#external_select",
                    "label": "Quote",
                    "name": "quote",
                    "url": "http://jsonplaceholder.typicode.com/posts/{{ external.post }}",
                    "optionValue": "id",
                    "optionText": "value",
                    "expression": "[{id: 'title', value: title}, {id: 'body', value: body}]"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#number",
                    "label": "Photo id",
                    "name": "photo_id",
                    "value": "",
                    "helptext": "Between 1 and 5000",
                    "conditions": "",
                    "decimals": "0",
                    "min": "1",
                    "max": "5000"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#external_data",
                    "name": "photo",
                    "url": "https://jsonplaceholder.typicode.com/photos/{{ external.photo_id }}",
                    "conditions": "external.photo_id != \"\""
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#checkbox",
                    "label": "{{ external.photo.title }}",
                    "name": "photo_checkbox",
                    "text": "{{ external.photo.url }}"
                }
            ]
        },
        {
            "label": "Conditional step",
            "group": "conditional_step",
            "conditions": "validation.show_next_step == 'show'",
            "fields": [
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Show fields",
                    "name": "show_fields",
                    "helptext": "Type 'show' to see the fields"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form-external/schema.json#external_select",
                    "label": "Post",
                    "name": "post",
                    "url": "http://jsonplaceholder.typicode.com/posts",
                    "optionValue": "id",
                    "optionText": "title",
                    "conditions": "conditional_step.show_fields == 'show'"
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
        },
        {
            "label": "Final step",
            "group": "final",
            "conditions": "validation.show_next_step == 'show'",
            "fields": []
        }
    ]
}
```
