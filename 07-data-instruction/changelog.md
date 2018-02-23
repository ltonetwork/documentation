### draft 01

* Data instructions are always objects with only the instruction as property. This removes the arbitrary order of execution.
* `<ref>` now takes a JMESPath query instead. Dot notation will also work as JMESPath query.
* Added `<equal>` which compares 2 values, resulting in a boolean.
* Added `<match>` which finds a substring or matches against a regex, resulting in a boolean.
* Removed `<ifset>`, use `<if>` instead
* Added `<if>` which takes a boolean as condition and has a `then` and `else` property.
* `<switch>` now is an object with `on` and `options`, rather than using additional properties in the instruction object.
* The `<enrich>` instruction now has an `input` property, rather than using additional properties in the instruction object.
* Replaced `<jmespath>` with `<apply>`. The `<apply>` instruction has an `input` and `query` property.
* `<merge>` no longer takes strings, use `<join>` for strings instead.
* Added `<join>` instruction to join strings.
* Added `<replace>` instruction to find / replace in a string.
* Added `<numberformat>` to format number using locale.
* `<dateformat>` may take `input_format` and `locale`.
* Renamed `date` property of `<dateformat>` to `input`.
* Removed `<transform>` instruction. Instead there are several new instructions.
* Added `<hash>` instruction. This supports less methods.
* A `<hash>` instruction can have an `hmac` property.
* Added `<encode>` and `<decode>` instructions.
* New encode / decode methods `base58` and `url`.
* Added `<serialize>` and `<unserialize>` instructions.
* No longer support the php serialize / unserialize methods.
* New serialize / unserialize method `url`. This creates an URL Query from an object.
* Added `<encrypt>` and `<decrypt>` instructions.
* Added `<sign>` and `<verify>` to create and verify a cryptographic signature.
