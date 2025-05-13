# Templates Overview

## Overview

Currently, the oBuilder creates Ownables with different feature sets for the users. Each different feature requires a different Ownable template for the internal LTO based flow of cosm/wasm Smart Contract compilation to reflect the required functionality.&#x20;

### Example of different functionalities

* Template 1: Ownable with a picture, thumbnail, name, description, keywords, owner
* Template 2: Ownable with a picture, thumbnail, name, description, keywords, owner + rwacontract.html (resides in parallel to the index.html file under assets directory)
* Template 3: (feature not enabled yet) Ownable with a picture, thumbnail, name, description, keywords, owner + audio file and/or short movie file&#x20;

### Templates folder structure

All templates are structured as Rust-based smart contracts with web assets:

```markup
template/
├── assets/ # Directory for web assets for the NFT
│ ├── index.html # Main HTML file
│ └── [other assets] # Template-specific assets
├── src/ # Rust smart contract code directory
│ ├── contract.rs # Main contract file
│ ├── error.rs # Errors file
│ ├── lib.rs # Library file
│ ├── msg.rs # Messages file
│ └── state.rs # State file
├── .cargo/ # Directory for rust configuration
│ └── config # Configuration file
├── examples/ # Directory for schema examples
│ └── schema.rs # Schema example
├── Cargo.toml # Cargo package manifest
└── rustfmt.toml # Rust formatting configuration
```

## Template Modifications

### Placeholder system overview

The templates use a placeholder system where specific tags in the code are replaced during the ownable creation process. These placeholders allow customization of the ownable's appearance, metadata, and functionality.

### Placeholders by File

#### Cargo.toml

Contains package metadata placeholders:

* PLACEHOLDER1\_NAME - Package name (e.g., "ownable\_my\_artwork")
* PLACEHOLDER1\_DESCRIPTION - Package description
* PLACEHOLDER1\_VERSION - Package version
* PLACEHOLDER1\_AUTHORS - Package authors
* PLACEHOLDER1\_KEYWORDS - Package keywords (as array)

```
[package]
name = PLACEHOLDER1_NAME
description = PLACEHOLDER1_DESCRIPTION
version = PLACEHOLDER1_VERSION
authors = [PLACEHOLDER1_AUTHORS]
edition = "2018"
keywords = [PLACEHOLDER1_KEYWORDS]
```

#### assets/index.html

Contains display/visual placeholders:

* PLACEHOLDER2\_TITLE - Title displayed in the browser tab
* PLACEHOLDER2\_IMG - Image file path for displaying the main image

```html
<html lang="">

<head>
	<title>PLACEHOLDER2_TITLE</title>
	...
	
<body>
	<div class="image-container">
		<img src=PLACEHOLDER2_IMG/>
	</div>
</body>
```

#### examples/schema.rs

Contains code module reference placeholders:

* PLACEHOLDER3\_MSG - Module path for message types
* PLACEHOLDER3\_STATE - Module path for state types

```rust
use PLACEHOLDER3_MSG::msg::{InstantiateMsg, ExecuteMsg, QueryMsg};
use PLACEHOLDER3_STATE::state::{Config};
```

#### src/contract.rs

Contains contract identity placeholders:

* PLACEHOLDER4\_CONTRACT\_NAME - Name of the smart contract
* PLACEHOLDER4\_TYPE - Type identifier for the ownable
* PLACEHOLDER4\_DESCRIPTION - Detailed description
* PLACEHOLDER4\_NAME - Display name

```rust
const CONTRACT_NAME: &str = PLACEHOLDER4_CONTRACT_NAME;
const CONTRACT_VERSION: &str = env!("CARGO_PKG_VERSION");

pub fn instantiate(
    deps: DepsMut,
    _env: Env,
    info: MessageInfo,
    msg: InstantiateMsg,
) -> Result<Response, ContractError> {
    set_contract_version(deps.storage, CONTRACT_NAME, CONTRACT_VERSION)?;

    let derived_addr = address_lto(
        msg.network_id as char,
        info.sender.to_string()
    )?;

    let ownable_info = OwnableInfo {
        owner: derived_addr.clone(),
        issuer: derived_addr.clone(),
        ownable_type: Some(PLACEHOLDER4_TYPE.to_string()),
    };

    let metadata = Metadata {
        image: None,
        image_data: None,
        external_url: None,
        description: Some(PLACEHOLDER4_DESCRIPTION.to_string()),
        name: Some(PLACEHOLDER4_NAME.to_string()),
        background_color: None,
        animation_url: None,
        youtube_url: None
    };
```

### How Modifications Work

The user changes the different PLACEHOLDER in the template file used to create the Ownable and the ownable will be unique to the user specified variables:

For example, when a user creates an Ownable with an image named "artwork.webp" with title "My Digital Artwork":

* PLACEHOLDER2\_TITLE becomes "My Digital Artwork"
* PLACEHOLDER2\_IMG becomes "artwork.webp"
* Other placeholders must be changed by the user in the same manner

This template system allows the same base templates to create unique ownables with different content, metadata, and appearance while maintaining consistent smart contract functionality.

### Placeholder Mapping Example

The oBuilder e.g. uses the following placeholder ownableData.json to create the Ownables. The user should use a similar approach.

```
[
  {
    "PLACEHOLDER1_NAME": "ownable_my_artwork",
    "PLACEHOLDER1_DESCRIPTION": "My amazing artwork description",
    "PLACEHOLDER1_VERSION": "0.1.0",
    "PLACEHOLDER1_AUTHORS": "John Doe",
    "PLACEHOLDER1_KEYWORDS": ["art", "digital", "nft"],
    "PLACEHOLDER2_TITLE": "My Digital Artwork",
    "PLACEHOLDER2_IMG": "artwork.webp",
    "PLACEHOLDER4_TYPE": "artwork",
    "PLACEHOLDER4_DESCRIPTION": "A detailed description of my artwork",
    "PLACEHOLDER4_NAME": "My Artwork NFT"
  }
]
```

### Template Differences:

* Template 1: Basic template with standard assets
* Template 2: Enhanced template with additional HTML file (rwacontract.html)
* Template 3: Alternative templates with different assets such as audio or short video files (Not yet implemented)
* More coming up...!

