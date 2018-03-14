[← back](../)

## Action

In a process an [action](../04-scenario/#action-schema) may be performed to trigger a state change or to update the
process projection. For each action in the scenario, the `$schema` property MUST be set.

The action type is determined by the `$schema` and used to execute an action of to display it correctly in the UI.

## Schemas

* [Manual action](#manual-schema)
* [Form action](#form-schema)
* [Upload action](#upload-schema)
* [Review action](#review-schema)
* [Edit action](#edit-schema)
* [Follow action](#follow-schema)

Automated actions

* [NOP action](#nop-schema)
* [HTTP action](#http-schema)
