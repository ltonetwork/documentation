### draft 01

* Rather than adding properties directly to the process object, they are now combined in the `info` property.
* Removed the `data` and `private_data` property.
* Added `description` to actors.
* Added `publickeys` to actors. This is a set of keys that belong to the actor.
* A public key has a from and until date.
* Assets are now more versatile. They can be anything, check the `$schema`.
* Removed `previous_response`. You can use `{ "$ref": "previous[-1].data" }` instead.
* Actions in `previous` have a `signature` and a `receipt`.
* Current is a `state` instead of an `action`.
* Next are `states` instead of `actions`.
* Current state has all possible actions (not just a reference).
* Action of current state may have multiple actors.
* Comments have a `signature` and a `receipt`.
