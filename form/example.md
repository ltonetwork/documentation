## Form example

```json
{
    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#",
    "id": "lt:/forms/a2c23ce9-bf80-4510-8df0-d7806e4a0f44",
    "title": "Demo form",
    "definition": [
        {
            "label": "All field types",
            "helptext" : "<h1>Overview of all field types</h1><p>These are the available field types</p>",
            "helptip" : "Please try every field",
            "fields": [
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Text",
                    "name": "text"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#password",
                    "label": "Password",
                    "name": "password"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#number",
                    "label": "Number",
                    "name": "number"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#amount",
                    "label": "Number with unit",
                    "name": "number_with_unit",
                    "optionValue": [
                        "unit"
                    ],
                    "optionText": [
                        "units"
                    ]
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#money",
                    "label": "Amount",
                    "name": "amount"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#date",
                    "label": "Date",
                    "name": "date"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#email",
                    "label": "E-mail",
                    "name": "email"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#textarea",
                    "label": "Text area",
                    "name": "textarea"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#select",
                    "label": "Select",
                    "name": "select",
                    "options": [
                        {
                            "value": "1",
                            "label": "one"
                        },
                        {
                            "value": "2",
                            "label": "two"
                        },
                        {
                            "value": "3",
                            "label": "three"
                        }
                    ]
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#group",
                    "label": "Option group",
                    "name": "option_group"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#checkbox",
                    "label": "Checkbox",
                    "name": "checkbox",
                    "text": "Yes or no"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#likert",
                    "label": "Likert",
                    "name": "likert",
                    "keys": [
                        "Do you like green?",
                        "Do you like blue?",
                        "Do you like red?"
                    ],
                    "values": [
                        "Hate it",
                        "Dislike it",
                        "Neutral",
                        "Like it",
                        "Love it"
                    ]
                }
            ]
        },
        {
            "label": "Expression",
            "group": "expression",
            "helptext": "<p>This step shows the use of expressions as well as templating labels and texts. Fill out the first name and you see it being used in the form.</p>",
            "fields": [
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "First name",
                    "name": "first_name"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Last name of {{ expression.first_name || '...' }}",
                    "name": "last_name"                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#expression",
                    "name": "name",
                    "expression": ".first_name + \" \" + .last_name"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#select",
                    "label": "I like {{ expression.name || '...' }}",
                    "name": "like",
                    "options": [
                        {
                            "value": "1",
                            "label": "yes"
                        },
                        {
                            "value": "0",
                            "label": "no"
                        }
                    ]
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#static",
                    "name": "static_data",
                    "content": "<p><strong>Entered user data:</strong></p><p><em>First name:</em> {{ expression.first_name }}</p><p><em>Last name:</em> {{ expression.last_name }}</p>",
                    "conditions": ".like == 1"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#group",
                    "label": "Option group",
                    "name": "option_group",
                    "options": [
                        {
                            "value": "1",
                            "label": "one"
                        },
                        {
                            "value": "2",
                            "label": "two"
                        },
                        {
                            "value": "3",
                            "label": "three"
                        }
                    ],
                    "conditions": ".like == 1"
                }
            ]
        },
        {
            "label": "Validation",
            "group": "validation",
            "helptext": "<p>Please fill out everything correctly to continue.</p>",
            "fields": [
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Required",
                    "name": "required",
                    "required": true
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#select",
                    "label": "Select required",
                    "name": "select_required",
                    "required": true,
                    "options": [
                        {
                            "value": "1",
                            "label": "one"
                        },
                        {
                            "value": "2",
                            "label": "two"
                        },
                        {
                            "value": "3",
                            "label": "three"
                        }
                    ]
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Pattern",
                    "name": "pattern",
                    "helptext": "Please enter a lower case letter followed by one or more digits",
                    "pattern": "[a-z]\\d+"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Mask",
                    "name": "mask",
                    "helptext": "4 digits, 2 letters",
                    "mask": "9999aa"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#password",
                    "label": "Password",
                    "name": "password",
                    "helptext": "Password should be at least 8 symbols long",
                    "pattern": ".{8,}"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#number",
                    "label": "Even",
                    "name": "even",
                    "helptext": "An even number",
                    "decimals": "0",
                    "validation": "validation.even % 2 === 0"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#number",
                    "label": "Min/Max",
                    "name": "min_max",
                    "helptext": "Between 3 and 7",
                    "decimals": "0",
                    "min": "3",
                    "max": "7"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#email",
                    "label": "E-mail",
                    "name": "email"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Show next step",
                    "name": "show_next_step",
                    "helptext": "Type 'show' to see the next step"
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#checkbox",
                    "label": "",
                    "name": "continue",
                    "text": "Please select this to continue",
                    "required": true
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
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Required",
                    "name": "required",
                    "conditions": "conditional_step.show_fields != 'show'",
                    "required": true
                },
                {
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#text",
                    "label": "Masked field",
                    "name": "masked_field",
                    "conditions": "conditional_step.show_fields == 'show'",
                    "mask": "999aa"
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
                    "$schema": "https://specs.livecontracts.io/v0.1.0/form/schema.json#checkbox",
                    "label": "{{ conditional_step.photo.title }}",
                    "name": "photo_checkbox",
                    "text": "{{ conditional_step.photo.url }}",
                    "conditions": "conditional_step.show_fields == 'show'"
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
