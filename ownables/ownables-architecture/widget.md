# Widget

The other side of the Ownable package will contain the visual elements: HTML, and media files like images and videos.

### HTML

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

### Styling

The styling should be responsive on all devices. As the wallet will render your provided template, you are in complete control of how it will be displayed.

The widgets in which the Ownables will be placed always have a 1:1 aspect ratio (square). Use the [viewport units](https://css-tricks.com/fun-viewport-units/) `vh` and `vw`, and/or [`aspect-ratio`](https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio) to ensure the content of the widget properly scales.

```css
.square {
  width: 50vw;
  height: 50vh;
  border: 1px solid black;
}

img.banner {
  width: 80vw;
  aspect-ratio: 4:1;
}
```

### Javascript

Ownable templates communicate with the wallet via messages.

An `execute` message can be sent to the window parent, which will be forwarded to the CosmWasm smart contract and stored on the event chain.

```javascript
window.parent.postMessage({
  type: "execute",
  ownable_id: ownable_id,
  msg: {
    "do_something: {}
  }
});
```

The widget will receive a message from the parent window after the state of the smart contract changes.

```javascript
window.addEventListener("message", (event) => {
  const state = event.data.state;
  document.getElementById("my_element").style.backgroundColor = state.color;
});
```

#### Example

If we consider the potion example which you can find in our demo repository, the script part of our HTML template may contain methods like this:

```javascript
// Listen for a click on the button defined in the html template
// which will trigger the consume method
document.getElementById("drink-button").addEventListener('click', () => consume());

function consume() {
  // building the message to be passed to WASM for execution
  // this should match the msg expected by the lib.rs entry points
  let msg = {
    "consume": {
      "amount": getDrinkAmount(),
    },
  };
  // posting the message to the wallet which will relay it to WASM
  // check index.js for the listener to see how it does it
  // https://github.com/ltonetwork/ownable-demo/blob/eb0bda9c323659a78d69bd669861f46473d16fa2/www/index.js#L163
  window.parent.postMessage({type: "execute", ownable_id, msg}, "*");
}

// gets the selected amount to drink from the html slider
function getDrinkAmount() {
  let stringAmount = document.getElementsByClassName('slider')[0].valueOf().value;
  return parseInt(stringAmount);
}

// listener for the wallet messages indicating the state after the execute msg
// see wasm-wrappers.js for details:
// https://github.com/ltonetwork/ownable-demo/blob/main/www/wasm-wrappers.js#L210-L217
window.addEventListener("message", (event) => {
  const state = event.data.state;
  updateTemplate(state.color, state.current_amount)
});

// method to update the visual template defined in the html file
function updateTemplate(color, amt) {
  document.getElementsByClassName('juice')[0].style.backgroundColor = color;
  document.getElementsByClassName('juice')[0].style.top = (100 - amt) + '%';
  document.getElementsByClassName('juice')[0].style.height = amt + '%';
  document.getElementsByClassName('amount')[0].textContent = amt;
}
```
