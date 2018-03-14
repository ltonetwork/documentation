### draft 01

* **Explicit states**
* Added `$schema` property, which can be consulted for the specification version.
* The scenario MUST support versioning.
* The `id` property is now an LTRI that MUST include the version number.
* Renamed the `alias` property to `keywords`. This can be anything to search on, not only alternative names.
* Added tagging with the `tags` property. A tag can be any type of string.
* Removed properties `published`, `personal_organization`, `notify_assets`, `notify_comments` and `locked`. Use `tags` instead.
* Removed `categories` property. Solve with either `tags` using an LTRI as tag `lt:/category/YCpKr0xp`.
* Removed `start` property. Instead the initial state is indicated with the `:initial` key.
* Removed `legalform` property. If the process is started with a form, that should be the action in the `:initial` state.
* Removed `contracts` property. Instead use the `scenarios` property in a template.
* Replaced `internal_description` with `instructions`. Instructions can be set per actor.
* Moved the `label` property from action to response.
* Moved the `display` property from action to response.
* A state without a default action an a `timeout` may specify a transition with a timeout response.

#### Explicit states

In the previous version, states were implicit. We would identify the current action and the current state always be
'waiting on user to perform current action'. Alternative actions were specific with the default action.

Now states are explicit. This means the same action is available from different states. Also, an action doesn't always
cause a state change.

The state definition declares all actions that are possible from that state. The `default_action` property is used to
determine the default action. This is useful for the golden flow. The default may be performed automatically (trigger)
or shown as default.

You may still define a state change and update instruction with an action. However you can also define a conditional
state change with states. This reduces the need of `nop` and `validation` triggers.

The previous events of a process consist of actions. For the process `current` and `next`, states are used.
