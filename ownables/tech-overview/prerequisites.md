# Prerequisites

#### Summary

The following dependencies will be required to develop Ownables:

* `rustc` and `cargo` versions 1.61.0 or later
* `npm` version 8.11.0 or later
* `node` version 16.15.0 or later
* `wasm-pack` version 0.10.2 or later

If you do not have the above dependencies on your machine, see the sections below for instructions.

### Rust and Cargo

Ownable packages are built with Rust.

To install Rust and its package manager Cargo, follow the official [documentation guide from the Rust Book](https://doc.rust-lang.org/cargo/getting-started/installation.html). On Linux and macOS systems, this is done as follows:

```shell-session
$ curl https://sh.rustup.rs -sSf | sh
```

Verify the installation by checking the version:

```shell-session
$ cargo version
```

The version should be 1.61.0 or later.

### Node & npm

`npm` will be needed for running the local wallet instance and building the Ownable packages.

Install it as described on the official [Node.js website](https://nodejs.org/en/download/).

Verify the installation with:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ npm -v
</strong>$ node -v
</code></pre>

The node version should be 16.15.0 or later. The npm version should be 8.11.0 or later.

### wasm-pack

The smart contracts written in Rust need to be compiled to WebAssembly. This way we can easily interact with them in our wallet.

`wasm-pack` makes building and working with rust-generated WASM easy.&#x20;

Follow the latest steps described in [their official documentation](https://rustwasm.github.io/wasm-pack/installer/) to install it. On Linux and macOS systems, this is done as follows:

```shell-session
$ curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
```

Once that is done, verify the installation:

```shell-session
$ wasm-pack -V
```

The version should be 0.10.2 or later.

### Code editor (optional)

Any editor you prefer will do the job, but there are a few recommendations worth looking into here.

First is the VSCode editor which you can download from the [official source](https://code.visualstudio.com/). Along with it, you may consider installing [this](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) Rust plugin to help with syntax highlighting and other amenities.

Another great pick is the [WebStorm by Jetbrains](https://www.jetbrains.com/webstorm/). JetBrains provides a Rust [plugin](https://intellij-rust.github.io/) which is very useful to help with the development flow.
