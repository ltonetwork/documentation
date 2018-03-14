[← back](../)

# Form

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
    "$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#",
    "id": "lt:/forms/0184c1a5-ecaa-4241-857c-74150d48f48d",
    "title": "Purchase",
    "definition": [
        {
            "label": "Register",
            "fields": [
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#text",
                    "label": "Name",
                    "name": "name"
                },
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#password",
                    "label": "Password",
                    "name": "password"
                },
            ]
        },
        {
            "label": "Order",
            "fields": [
                {
                    "$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#number",
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

### name

The name of the form.

### locale

The ISO-639 locale for the form. This determines the input format for numbers and dates.

### definition

The array of [steps](#step-schema).

## apply_in

A [JMESPath query](http://jmespath.org/) to transform data which is offered as input to the form.

## apply_out

A [JMESPath query](http://jmespath.org/) to transform data when outputted from the form.

## Step schema

A form has steps which groups the fields. Steps can be shown one at a time, like a wizard, or all at once.

[JSON Schema](schema.json#step)

### label

Step label or header shown above the fields of the step.

### group

Variable name to group fields. If specified, the step creates an object as variable. All fields of the step are
properties of the object.

### repeat

A number, field reference or expression determining how often the step is repeated when filling out the form.

If the step is repeated, specifying a `group` is required. In that case the step creates an of objects instead.

### anchor

If the form is associated to a template, enter a paragraph number or anchor name. The UI SHOULD scroll to the anchor
when this step is focused.

### helptext

HTML that is shown when filling out this step. Typically this is shown in place of the preview.

### helptip

A short text with instructions that is shown for the step.

### conditions

An expression to show / hide the step. If the expression validates to `true` the step is show, when `false` the step
MUST be hidden.

If the step is hidden, the variables connected to the fields in this step remain undefined.

### fields

An array of field definitions. Each field is structured according to the schema of a specific field type.

## Common field properties

A number of properties are shared across many of the field types.

### $schema

The JSON Schema of the field. This identifies the field type.

### label

A label that is shown with the field.

The label is rendered as [mustache template](https://mustache.github.io/) and may reference data variables.

### name

The variable name for the field. When filling out the form, a variable with this name will contain the typed in value.
This is also the name used for the result data.

You may use a dot to group data (eg `client.name`). An object is automatically created in this case. This can be used in
addition or instead of `group` in a step.

### default

The default value of the field.

### helptext

Help text attached to the field as HTML. This help text may contain specific instructions about filling out this field.

The help text is rendered as [mustache template](https://mustache.github.io/) and may reference data variables.

### conditions

An expression to show / hide the step. If the expression validates to `true` the step is show, when `false` the step
MUST be hidden.

If the field is hidden, the variable connected to the fields remains undefined. You MAY use the same `name` for
different fields, as long as only one is shown at any time.

### required

Boolean value to set the field as required or not. If a required field is not filled out, it will result in a validation
error.

### min

The minimal value of the field. A value lower than this results in a validation error.

### maximum

The minimal value of the field. A value higher than this results in a validation error.

### validation

An expression that is used for validation. This expression may reference any field of the form.

## Text schema

[JSON Schema](schema.json#text)

A one-line text input field. In HTML it MAY be rendered as

    <input type="text">

### $schema

For a text field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#text"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### pattern

A regular expression to validate the value. If the value doesn't match the pattern, it will result in a validation
error.

### mask

An input mask for the field. The use isn't able to enter any characters that don't match the mask. The following chars
are available:

```
* - matches a single character
A - matches a letter (a-z or A-Z)
9 - matches a number (0-9)
```

### validation

[See common field properties](#common-field-properties)

## Password schema

[JSON Schema](schema.json#password)

A password input field. In HTML it MAY be rendered as

    <input type="password">

### $schema

For a password field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#password"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### pattern

A regular expression to validate the value. If the value doesn't match the pattern, it will result in a validation
error.

### validation

[See common field properties](#common-field-properties)

## Number schema

[JSON Schema](schema.json#number)

A number input field. In HTML it MAY be rendered as

    <input type="number">

### $schema

For a number field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#number"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### decimals

The number of decimals the value may have. Defaults to 0 decimals, so an integer. If more decimals are specified in the
value, it is rounded.

### min

[See common field properties](#common-field-properties)

### max

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## Amount schema

[JSON Schema](schema.json#amount)

The amount field is a number with a unit as suffix. Example `3 hours` or `17 products`. The value is an object with an
`amount` and a `unit` property, which may be rendered to a string.

### $schema

For an amount field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#amount"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### decimals

The number of decimals the value may have. Defaults to 0 decimals, so an integer. If more decimals are specified in the
value, it is rounded.

### min

[See common field properties](#common-field-properties)

### max

[See common field properties](#common-field-properties)

### options

An array of options for the unit. Each option is defined as object with a `singular` and `plural` properties.

```json
{
  "options": [
    { "singular": "minute", "plural": "minutes" },
    { "singular": "hour", "plural": "hours" },
    { "singular": "day", "plural": "days" }
  ]
}
```

If there is only one option in the array it's automatically selected, otherwise you can pick one of the options using a
dropdown.

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## Money schema

[JSON Schema](schema.json#money)

The money field is a numeric input with fixed currency prefix. When cast to a string, the value is automatically
prefixed with the currency symbol.

The currency is constant for the whole form and determined by a field named `currency`. If you want to the user to set
the currency for the form, use a [select field](#select-schema). Alternatively you can set the currency using an
expression.

_If you need to choose the currency per field, use the [amount field](#amount-schema) instead._

### $schema

For a money field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#money"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### min

[See common field properties](#common-field-properties)

### max

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## Date schema

[JSON Schema](schema.json#date)

An input field for a date. The value of the date is always in ISO-8601 form. The input depends on `locale` set for the
form.

### $schema

For a date field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#date"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### min

[See common field properties](#common-field-properties)

### max

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## email schema

[JSON Schema](schema.json#email)

An input field for an email address. In HTML this MAY be rendered as

    <input type="email">

### $schema

For an email field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#email"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## textarea schema

[JSON Schema](schema.json#textarea)

A multi-line text input control. In HTML this MAY be rendered as

    <textarea>

### $schema

For a textarea field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#textarea"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## select schema

[JSON Schema](schema.json#select)

Input control to select on of the specified options. In HTML this MAY be rendered as

    <select>
      <option></option>
      ...
    </select>

### $schema

For a select field use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#select"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

The default value of the select field. Depending if multiple items may be selected, this should be a single value or an
array of values. The value of an option can be a string, integer or number.

### options

The options that a user may select from as array. Each item in the array is an object with a `value` and `label`
property. The label is shown, while the value is set to the variable.

An option may also have a `condition` property, which is used to determine if the option is shown or not.

```json
{
  "options": [
    { "value": "#ff0000", "label": "red" },
    { "value": "#00ff00", "label": "green" },
    { "value": "#0000ff", "label": "blue" },
    { "value": "#ffff00", "label": "yellow", "condition": "secondary_colors == true" },
    { "value": "#ff00ff", "label": "pink", "condition": "secondary_colors == true" }
  ]
}
```

### multiselect

Boolean to indicate multiple values can be selected. Set to `false` by default.

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

If the field is not required, an option SHOULD be added to select and empty value.

### validation

[See common field properties](#common-field-properties)

## select group schema

[JSON Schema](schema.json#group)

Checkbox or radio button group to select on of the specified options. This works similar to a `select` field, only it
SHOULD be rendered differently.

### $schema

For a checkbox / radio button group use
`"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#select-group"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### default

The default value of the select field. Depending if multiple items may be selected, this should be a single value or an
array of values. The value of an option can be a string, integer or number.

### options

The options that a user may select from as array. Each item in the array is an object with a `value` and `label`
property. The label is shown, while the value is set to the variable.

An option may also have a `condition` property, which is used to determine if the option is shown or not.

```json
{
  "options": [
    { "value": "#ff0000", "label": "red" },
    { "value": "#00ff00", "label": "green" },
    { "value": "#0000ff", "label": "blue" },
    { "value": "#ffff00", "label": "yellow", "condition": "secondary_colors == true" },
    { "value": "#ff00ff", "label": "pink", "condition": "secondary_colors == true" }
  ]
}
```

### multiselect

Boolean to indicate multiple values can be selected. Set to `false` by default.

Without multiselect this MAY be rendered as radio buttons. With multiselect this MAY be rendered as checkboxes.

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

If the field is not required, an option SHOULD be added to select and empty value.

### validation

[See common field properties](#common-field-properties)

## checkbox schema

[JSON Schema](schema.json#checkbox)

A single checkbox. In HTML this MAY be rendered as

    <input type="checkbox">

### $schema

For a checkbox use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#checkbox"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### checked

Boolean to indicate if the field should be checked by default.

### value

The value given to the variable if the checkbox is selected. Defaults to `true`.

### text

The text displayed with the checkbox.

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### required

[See common field properties](#common-field-properties)

If the field is not required, an option SHOULD be added to select and empty value.

### validation

[See common field properties](#common-field-properties)

## likert schema

[JSON Schema](schema.json#likert)

A [like scale](https://en.wikipedia.org/wiki/Likert_scale) input. This is a grid input with questions and options. The
options are the same for each question.

This is typically used in a questionary to ask for a level of agreement/disagreement. 

The (output) value of the likert MUST be an array with the selected values for each question. If a question is not
answered the value is `null`.

### $schema

For a likert use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#likert"`.

### label

[See common field properties](#common-field-properties)

### name

[See common field properties](#common-field-properties)

### keys

A set of questions. For each question a value is picked.

### options

The options that a user may select from as array. Each item in the array is an object with a `value` and `label`
property. The labels are shown as column headers. The value of variable is an array with the value for each selected
option per question.

```json
{
  "options": [
    { "value": -2, "label": "Strongly disagree" },
    { "value": -1, "label": "Disagree" },
    { "value": 0, "label": "Neither agree nor disagree" },
    { "value": 1, "label": "Agree" },
    { "value": 2, "label": "Strongly agree" },
  ]
}
```

### helptext

[See common field properties](#common-field-properties)

### conditions

[See common field properties](#common-field-properties)

### validation

[See common field properties](#common-field-properties)

## expression schema

An expression is not rendered as a field, instead the expression is calculated in the background. It typically refers
to other variables in the form.

### $schema

For an expression use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#expression"`.

### name

[See common field properties](#common-field-properties)

### expression

The expression that is calculated to yield the value.

```json
{
  "name": "full_name",
  "expression": "first_name + ' ' + last_name"
}
```

### static schema

Static HTML, for display purpose only.

### $schema

For static HTML use `"$schema": "http://specs.livecontracts.io/draft-01/08-form/schema.json#static"`.

### content

HTML content as [mustache template](https://mustache.github.io/). The content may include variable like
`{{ full_name }}` referencing data variables.

### conditions

[See common field properties](#common-field-properties)
