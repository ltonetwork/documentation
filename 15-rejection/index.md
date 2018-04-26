# index

[← back](../)

## Rejection

A node is required to validate if an event is valid before adding it to the event chain. Events that are invalid may be rejected.

### Note to all readers

**Live Contracts draft 01 does not yet include specifications for rejects. Events that don't validate are silently dropped. This may give problems in Live Contracts implementations that support distributed contracts.**

### Validation

Each event SHOULD be validated before adding it to the event chain. This means verifying that

* The signature of the event is valid
* The hash of the event matches
* The event is less than 15 minutes old and has an receipt
* The event receipt can be verified

