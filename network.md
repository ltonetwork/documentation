[← back](./)

# Network

Live Contracts relies on message brokers to share [event chains](01-event-chains/) between nodes. The message broker
MUST use the [`AMQP 0-9-1`](https://www.rabbitmq.com/resources/specs/amqp0-9-1.pdf) to communicate and SHOULD be
available over a secured connection.

New events on a chain are pushes to all nodes designated by the participating [identities](02-identities/). Nodes may
also [request an event chain](14-chain-requests/) for instance when a user of the node is invited on a chain.

## Decoupling

A message broker allows for asynchronous and decoupled data transfer. With a protocols like HTTP, an unresponsive node
could slow down other nodes. With a message broker, messages to an unreachable node may be delivered as a later time.

## Recommended message brokers

* [RabbitMQ](https://www.rabbitmq.com/)

See [this list of client and developer tools](https://www.rabbitmq.com/devtools.html) that support `AMQP 0-9-1`.

## HTTP REST API

Nodes MAY have an HTTP REST API which can be used to interface with the event chain service. Other node SHOULD NOT use
the REST API. It's only intended for client applications hosted by the owner of the node.
