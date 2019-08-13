# Conflict resolution

In any decentralized system conflicts can arise. Multiple nodes may add an event at the same time or a node might miss an event causing it to branch the chain. Because an event represents something that happened off-chain, we can't simply drop events instead they need to be added in such a way that it's visible that a conflict occurred.

## Chain branches

If there is a conflict on an event chain, two \(or more\) branches of the chain exist. Conflict resolution means merging those branches back to a single event chain.

An identity can start a branch after the last event it has signed. This is a point up until we can prove that the identity has received the events. Trying to branch before this point MUST result in a [rejection](), resulting in a fork.

### Rebasing

To merge branches, events of one of the branches are rebased onto to other branch. A rebased event contains an `original` property with the properties of the original event, minus the body \(which doesn't change\).

### Rebuilding projections

If your branch is abandoned and you're switching to a new branch, your node needs to revert changes to projections up to the fork. For some projections like processes, it difficult to revert a state change.

Instead, the whole projection will be deleted and rebuild by replaying all events.

## Resolution

Every event is anchored on the public chain. In case of conflict, the anchoring transactions of the two conflicting events are retrieved. Transactions on the public chain are processed sequentially \(and not concurrently\), allowing us to determine which anchor transaction was processed first.

If the transactions are in different blocks, the block height determines which event happened first. If both events are in the same block, the transaction order of the block must be followed.

The node of the identity that created the conflicting event on the loosing branch, MUST rebase this event and any subsequent events onto the winning branch and broadcast these to the other nodes. All other nodes only switch to the winning branch. If a node is already on the winning branch, no action is taken.

