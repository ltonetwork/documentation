# Ownable Architecture

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

### Widget

The other side of the Ownable package will contain the visual elements: HTML, and media files like images and videos.

#### HTML

The `index.html` file will be rendered as widget in the wallet. It should have a structure resembling the following:

```html
<html>
  <head>
    <style>
      // all the styles for your ownable
    </style>
  </head>
  <body>
    <div>
      // your ownable html
    </div>
    <script>
      // any necessary functions
    </script>
  </body>
```

{% hint style="warning" %}
The html template should not use external javascript or css files.
{% endhint %}

#### Styling

The styling should be responsive on all devices. As the wallet will just render your provided template, you are in complete control of how it will be displayed.

The widgets in which the Ownables will be placed in will always have a 1:1 aspect ratio (square).

For consistency we suggest placing all action buttons unique to your ownable at the bottom of the template.

Keep in mind that during the instantiation process the wallet will inject the general action buttons menu at the top-right of the container. It will have the following styling:

```css
.three-dots {
  top: 2%;
  right: 2%;
  padding: 1vw;
  position: absolute;
}
```

Make sure that it does not conflict with your styles.

#### Javascript

Ownable templates communicate with the wallet via messages. If we consider the potion example which you can find in our demo repository, the script part of our html template may contain methods like this:

```javascript
// waiting for the click to consume our potion
document.getElementById("drink-button").addEventListener('click', () => consume());

// method to update the visual template
function updateTemplate(color, amt) {
  document.getElementsByClassName('juice')[0].style.backgroundColor = color;
  document.getElementsByClassName('juice')[0].style.top = (100 - amt) + '%';
  document.getElementsByClassName('juice')[0].style.height = amt + '%';
  document.getElementsByClassName('amount')[0].textContent = amt;
}

window.addEventListener("message", (event) => {
  const state = event.data.state;
  updateTemplate(state.color, state.current_amount)
});

function consume() {
  // building the message to be passed to WASM for execution
  let msg = {
    "consume": {
      "amount": getDrinkAmount(),
    },
  };
  // posting the message to the wallet which will relay it to WASM
  window.parent.postMessage({type: "execute", ownable_id, msg}, "*");
}
// returns the selected consumption amount
function getDrinkAmount() {
  let stringAmount = document.getElementsByClassName('slider')[0].valueOf().value;
  return parseInt(stringAmount);
}
```
