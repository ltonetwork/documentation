# Smart Contract

The architecture of Ownables follows that of a general CosmWasm contract. The only exception is that instead of deploying the contract on-chain and interacting with it that way, Ownable contracts are executed locally.

The wallet expects some methods from `ownable-std` to be used.

### Rust



**`msg.rs`**

Message file defines the expected interface for messages that are passed to our smart contract. You are free to introduce your own custom variants and fields for each message, but there are some macros provided by the `ownable-std` crate.

Lets look at an example with all macros enabled:

<pre class="language-rust"><code class="lang-rust">use cosmwasm_std::{Addr};
use ownable_std::NFT;
use schemars::JsonSchema;
use serde::{Deserialize, Serialize};
use ownable_std_macros::{
    ownables_transfer, ownables_lock,
    ownables_query_info, ownables_query_locked, ownables_query_metadata,
    ownables_query_consumer_of, ownables_query_widget_state,
    ownables_instantiate_msg
};

#[ownables_instantiate_msg]
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InstantiateMsg {}

#[ownables_transfer]
#[ownables_lock]
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {}
<strong>
</strong><strong>#[ownables_query_info]
</strong>#[ownables_query_locked]
#[ownables_query_metadata]
#[ownables_query_consumer_of]
#[ownables_query_widget_state]
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {}
</code></pre>

Procedural macros like `#[`ownables\_instantiate\_msg`]` will introduce properties expected by the Ownable architecture. In the instantiation case, it would merge the `InstantiateMsg` defined by you with all its properties with the following:

```rust
struct InstantiateMsg {
    pub ownable_id: String,
    pub package: String,
    pub nft: Option<NFT>,
    pub ownable_type: Option<String>,
    pub network_id: u8,
}
```

For the `ExecuteMsg` case, having the #\[ownables\_transfer] and #\[ownables\_lock] will produce the following enum:

```rust
pub enum ExecuteMsg {
    Transfer { to: Addr },
    Lock {},
}
```

And for `QueryMsg`, the following:

```rust
pub enum QueryMsg {
    GetInfo {},
    GetMetadata {},
    GetWidgetState {},
    IsLocked {},
    IsConsumerOf { issuer: Addr, consumable_type: String, },
}
```



If you are curious about the properties/variants being introduced, feel free to take a look at [ownable-std](https://github.com/ltonetwork/ownable-std/blob/main/macros/ownable-std-macros/src/lib.rs).

A few things are important to note that apply to all cases. First, ownable-std macro annotations should be declared above any derive and serde macros (as in the examples above).

Second, you are free to introduce any variants/fields that are necessary for your ownable.

Consider the following `ExecuteMsg`:

```rust
#[ownables_transfer]
#[ownables_lock]
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    ChangeColor { new_color: String, },
} 
```

&#x20;After compiling, the `ExecuteMsg` will look like the following:

```rust
pub enum ExecuteMsg {
    ChangeColor { new_color: String, },
    Transfer { to: Addr },
    Lock {},
}
```

**`contract.rs`**

In the contract file, we define the logic that will update and query the state of our contract.

The `instantiate()` method should extract the necessary properties from its arguments and save them into the appropriate `cw_storage_plus::Item` defined in `state.rs`. If everything goes well, we return the `Ok()` response with all the relevant properties that have been saved to the host wallet.

`execute()` will contain any functions that the Ownable may contain. At the very least, having a transfer method is expected, but even that is technically optional if you want your Ownable to be non-transferable. Any logic surrounding that should be implemented by you. With the appropriate `Item.update()` , the state will be updated to reflect the results of execution (if any).

Usually in your `execute()` you will need to exhaustively match all variants available in `ExecuteMsg` defined in our `msg.rs` file. Given a transferable and lockable ownable, it may make sense to route the execution to respective methods:

```rust
match msg {
        tExecuteMsg::Transfer { to } => try_transfer(info, deps, to),
        ExecuteMsg::Lock {} => try_lock(info, deps),
}
```

`try_transfer` and `try_lock` are the methods where you will define your application logic related to transfering and locking the ownable.

With `query()` method we query the contract state. `Item.load()` will load the existing state variables, which you can then use to return the desired type (in the expected binary format). Considering our `QueryMsg` definition above, our query entry point may look like this:

```rust
pub fn query(deps: Deps, _env: Env, msg: QueryMsg) -> StdResult<Binary> {
    match msg {
        QueryMsg::GetInfo {} => query_ownable_info(deps),
        QueryMsg::GetMetadata {} => query_ownable_metadata(deps),
        QueryMsg::GetWidgetState {} => query_ownable_widget_state(deps),
        QueryMsg::IsLocked {} => query_lock_state(deps),
        QueryMsg::IsConsumerOf {
            issuer,
            consumable_type
        } => query_is_consumer_of(deps, issuer, consumable_type),
    }
}
```

Once again, logic in methods like `query_ownable_info` is entirely up to you. In general they will involve loading the appropriate state variables, building your return object, and converting it to `StdResult<Binary>`.&#x20;

**`state.rs`**

We use the state to define what properties our Ownables should have.

In most cases you will only be required to define your own `Config` struct containing any properties specific to your design.

Other than that, you should store the following properties to allow your Ownable to be bridged, have metadata (defined in the [CW721 spec](https://github.com/CosmWasm/cw-nfts/blob/main/packages/cw721/README.md)), and to have the unlockable content:

```rust
pub const OWNABLE_INFO: Item<OwnableInfo> = Item::new("ownable_info");
pub const METADATA: Item<Metadata> = Item::new("metadata");
pub const NFT_ITEM: Item<NFT> = Item::new("nft");
pub const LOCKED: Item<bool> = Item::new("locked");
pub const PACKAGE_CID: Item<String> = Item::new("package_cid");
pub const NETWORK_ID: Item<u8> = Item::new("network_id");
```

Properties like `OwnableInfo`, `Metadata`, and `NFT` should be imported from the `ownable-std` package.

**`lib.rs`**

`lib.rs` defines the entry point into our contract logic. It accepts and produces JSON (`JsValue` or `JsError`), which the wallet/dApp expects and interprets.&#x20;

In turn, `lib` methods will call the respective `contract.rs` methods that contain the actual execution logic.

{% hint style="warning" %}
The code here should not change, as the wallet expects the functions to have the parameters and return values as they are defined.
{% endhint %}

