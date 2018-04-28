# draft 01

* Form definition holds a `$schema`.
* Step property `article` is renamed to `anchor`.
* Replaced field `type` with field `$schema`.
* The `amount` field now has an `options` property. This is an array of objects with a `singular` and `plural` property.
* Removed `exernal` property from `select` field. Use `external_select` instead.
* Moved select from external source and `external_data` to own specification `form-external`.
* Renamed `group` field to `select-group`.
* The `select-group` field has an `options` property, instead of `optionsValue` and `optionsText`.
* The `select-group` field must also support `default`.
* Added `condition` to the `options` of `select` and `select-group` fields.
* The `likert` field uses arrays for the `keys` and `options` properties rather than multiline text.
* The `options` property of the `likert` field has values and labels. The labels are shown as column head.
* Use the `default` property for the default value instead of `value`.
* Added `name` property to the form.
* Added `locale` property to the form which determines the number and date input format.
* Added `checked` property to a `checkbox` field.
* Renamed `jmespath_in` to `apply_in`.
* Renamed `jmespath_out` to `apply_out`.

