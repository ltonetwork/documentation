# Live Contracts - Form

A Live Contracts form defines an input form with steps containing fields. Filling out fields results in a data set. The
form and fields can be rendered directly to HTML or to JavaScript enabled widgets as with Angular or React.

## Schemas

* [Form](#form-schema)
* [Step](#step-schema)
* [Text field](#text-schema)
* [Password field](#password-schema)
* [Number field](#number-schema)
* [Amount field](#amount-schema)
* [Money field](#money-schema)
* [Date field](#date-schema)
* [E-mail field](#email-schema)
* [Textarea field](#textarea-schema)
* [Select field](#select-schema)
* [Checkbox field](#checkbox-schema)
* [Group field](#group-schema)
* [Likert field](#likert-schema)
* [Expression](#expression-schema)
* [Static](#static-schema)

## Example

```json
{
    "$schema": "http://specs.livecontracts.io/draft-01/05-form/schema.json#",
    "id": "lt:/forms/0184c1a5-ecaa-4241-857c-74150d48f48d",
    "title": "Purchase",
    "definition": [
        {
            "label": "Register",
            "fields": [
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/05-form/schema.json#text",
                    "label": "Name",
                    "name": "name"
                },
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/05-form/schema.json#password",
                    "label": "Password",
                    "name": "password"
                },
            ]
        },
        {
            "label": "Order",
            "fields": [
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/05-form/schema.json#number",
                    "label": "Amount",
                    "name": "amount"
                }
            ]
        }
    ]
}
```

### Full example

[See the full example with all field types](example.json)

## Form schema

[JSON Schema](schema.json#)

### $schema

The Live Contracts Event chain [JSON schema](http://json-schema.org) URI that describes the JSON structure of the event
chain. To point to this version of the specification use
`"$schema": "http://specs.livecontracts.io/draft-01/01-event-chain/schema.json#"`.

### id

A URI as a globally unique identifier for the form. This is typically an [LTRI](../00-ltri/)
using a random UUID-4; `lt:/forms/<uuid-4>`.

### definition

The array of steps.

## Step schema

A form has steps which groups the fields. Steps can be shown one at a time, like a wizard, or all at once.

[JSON Schema](schema.json#step)

### label

Step label or header shown above the fields of the step.

### group

Variable name to group fields. If specified, all

### anchor

### helptext

### helptip

### conditions

### fields
