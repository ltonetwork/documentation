# Live Contracts - Conflict resolution

In any decentralized system conflicts can arise. Multiple nodes may add an event at the same time or a node might miss
an event causing it to branch the chain. Because an event represents something that happened off-chain, we can't simply
drop events instead they need to be added in such a way that it's visible that a conflict occurred.

## Schema

* []

## Chain branches

If there is a conflict on an event chain, two (or more) branches of the chain exit. Conflict resolution means merging
those branches back to a single chain.

### Stitching

To merge branches, events of one of the branches are stitched onto to other branch. A stitch event is a wrapper around
the original event. Only the original identity is allowed to stich an event.

If a node detects that the chain has branched, it MAY choose to send a stitched event. Alternatively it may request
the other node to resolve the conflict by stitching its event(s). Last a node may do nothing 

### Resolution request



## Resolution methods

Every node is free to resolve conflicts any way it wants. Behaving in an unexpected way result in a dead lock or dead
loop. Therefor there's a recommended way of resolving conflicts.

In some cases it's possible to benefit by abusing conflict and the recommended way for conflict resolution. This can be
a reason to not adhere to this method.

### Recommended resolution



### Dominant identity

An alternative is that one identity is dominant. In case of a conflict, the event chain hold by that identity is
considered to be the valid one and other events are always stitched upon this. Such behavior is configured in the node.
Note that having multiple identities claim dominance can lead to a dead-lock. An identity SHOULD NOT behave dominant if
it did not initiate the event chain.

