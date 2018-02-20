### Live Contracts Forms - draft 01

* Form definition holds a `$schema`.
* Step property `article` is renamed to `anchor`.
* Replaced field `type` with field `$schema`.
* The `amount` field now has an `options` property. This is an array of objects with a `singular` and `plural` property.
* Removed `exernal` property from `select` field. Use `external_select` instead.
* Moved select from external source and `external_data` to own specification `09-form-external`.
* The `group` field has an `options` property, instead of `optionsValue` and `optionsText`.
* The `group` field must also support `options_selected`.
* The `likert` field uses arrays for the `keys` and `values` properties rather than multiline text.


### LegalForms

* Initial format
