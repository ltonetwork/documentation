# Live Contracts specifications

_Contracts and laws are not something static, but something that is dynamic and can be automation and allow for
participation and are understandable by people without legal knowledge._

### Note to Readers

The issues list for this draft can be found at <https://github.com/legalthings/livecontracts-specs/issues>.
For additional information, see <http://livecontracts.io/>.

To provide feedback, use this issue tracker, the communication methods listed on the homepage, or email the document
editors.

### Copyright Notice

Copyright (c) 2017 LegalThings One. All rights reserved.

You may use this specification under the [Creative Commons Attribution 4.0 International Public License](https://raw.githubusercontent.com/legalthings/livecontracts-specifications/master/LICENSE).

## General

### Cryptography

Live Contracts uses the `SHA256`, `Blake2b256` and `Keccak256` algorithms to create a cryptographic hashes. The
`Curve25519` (ED25519 with X25519 keys) scheme is applied to create and verify signatures and to encrypt and decrypt
data. `Base58` is used to create the string from of bytes.

[Read more](http://specs.livecontracts.io/cryptography.html)

## Specifications

The Live Contracts specifications are described as [JSON schema](http://json-schema.org/).

### 00 - LegalThings Resource Identifier (LTRI)

The LTRI follows a RESTful path structure to identify a resource. The resource collection is always plural. The LTRI
does not identify the system, but it identifies a resource within a system.

[Read more](http://specs.livecontracts.io/00-ltri/)

### 01 - Event chain

The event chain is a miniature blockchain that is shared between parties involved in a contract or process. Each event
is signed and that added referencing the previous event, forming a chain. All information of the system is derived from
event chains.

[Read more](http://specs.livecontracts.io/01-event-chain/)

### 02 - Identity

An identity within the event chain. The identity is used for authentication and authorization. An identity is not a
user, a user has an identity for each event chain it's participating in.

[Read more](http://specs.livecontracts.io/02-identity/)

### 03 - Template

The Live Contract template can be instantiated to create a contract. It holds a templated version of the natural
language text, a form definition to collect the data and links to associated scenarios of the digitized procedures.

[Read more](http://specs.livecontracts.io/03-template/)

### 04 Scenario

A Live Contract scenario is a definition of procedure as a Finite State Machine (FSM). As an FSM, the scenario can be
visualized as flowchart and instantiated as a process.

[Read more](http://specs.livecontracts.io/04-scenario/)

### 05 - Form

[Read more](http://specs.livecontracts.io/05-form/)

### 06 - Form external

[Read more](http://specs.livecontracts.io/06-form-external/)

### 07 - Data instruction

Data instructions allow you to dynamically set properties of a state or actions when they are instantiated using
data from the projected process.

[Read more](http://specs.livecontracts.io/07-data-instruction/)

### 08 - Contract

[Read more](http://specs.livecontracts.io/08-contract/)

### 09 - Process

[Read more](http://specs.livecontracts.io/09-process/)

### 10 - Action

[Read more](http://specs.livecontracts.io/10-action/)

### 11 - Comment

[Read more](http://specs.livecontracts.io/11-comment/)
