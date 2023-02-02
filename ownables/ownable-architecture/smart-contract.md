# Smart Contract

The architecture of Ownables follows that of a general CosmWasm contract. The only exception is that instead of deploying the contract on-chain and interacting with it that way, Ownable contracts are executed locally.

### Rust

**`contract.rs`**

In the contract file, we define the logic that will update and query the state of our contract.

The `instantiate()` method should extract the necessary properties from its parameters and save them with `CONFIG.save()`. If everything goes well, we return the `Ok()` response with all the relevant properties that have been saved to the host wallet.

`execute()` will contain any functions that the Ownable may contain. At the very least, having a transfer method is expected. Any logic surrounding that should be implemented by you. With `CONFIG.update()`, the state will be updated to reflect the results of execution.

With `query()` method we query the contract state. `CONFIG.load()` will load the existing state, which you can then use to return the desired `state` type (in the expected binary format).

**`state.rs`**

We use the state to define what properties our Ownables should have.

The basic metadata you may be interested in is defined in the [CW721 spec](https://github.com/CosmWasm/cw-nfts/blob/main/packages/cw721/README.md). Alongside that, you may add any properties specific to your Ownable design.&#x20;

**`lib.rs`**

`lib.rs` defines the entry point into our contract logic. It accepts and produces JSON (`JsValue` or `JsError`), which the wallet/dApp expects and interprets.&#x20;

In turn, `lib` methods will call the respective `contract.rs` methods that contain the actual execution logic.

{% hint style="warning" %}
The code here should not change, as the wallet expects the functions to have the parameters and return values as they are defined.
{% endhint %}

**`store.rs`**

The store is used to interpret the WASM memory. As the wallet will be passing the byte arrays that describe the Ownable state, methods in `store.rs` will load that memory so that it becomes operational. In the case of `instantiate` and `execute`, store will also convert the existing WASM memory into a format that the wallet will be able to save.

_Typically store code should not require any customization._

**`utils.rs`**

The utils contain methods that help with the environment initialization and loading.

_Typically, no customization should be required here._

