# FAQ

## Frequently Asked Questions

### 1. How much LTO do I need to have in order to start the mining process?

In order to activate mining, the node needs not less than 1000 LTO (for testnet we can always provide you with that, do not hesitate to contact us).

### 2. Where can i get the link for last mainnet state?

[_**Last Mainnet State.**_](https://nodes.lto.network)

### 3. What is the incentive to run my own full node instead of leasing my coins?

More revenue if you have enough people leasing to you and payout is not 100%. Also you can provide services on LTO Network with your node. Running a node might be profitable later on but it's not at the moment. If it is it's only marginal.

### 4. I want to automatically send my tokens to multiple wallets on the LTO platform. Is there a program or bot?

Yes, there are payout scripts that node owners can use to pay leasers. These can also be used/adjusted to do other automated transfers, e.g., [_**LTOLPoSDistributor**_](https://github.com/jayjaynl/LTO\_LPoSDistributor). These scripts are provided and maintained by LTO community members.

### **5. How can I list my community node?**

The wallet uses a static JSON file to list the community nodes for leasing. Please edit the file [communityNodes.json](https://github.com/ltonetwork/lto-wallet/blob/master/src/communityNodes.json) on GitHub and issue a pull request.

In the future, services will pull the information from the blockchain. Please use a [data transaction](../../protocol/public/transactions/data.md) to set the meta properties for your node address.

{% tabs %}
{% tab title="CLI" %}
Store the node information in a JSON file.

```json
{
  "node_name": "My node name",
  "node_description": "Full description about your node",
  "branding:logo_svg": "https://example.com/logo.svg",
  "website": "https://example.com",
  "social:telegram": "@telegram_handle",
  "social:twitter": "@twitter_handle",
  "payout_sharing": "95%",
  "payout_schedule": "Every week"
}
```



Install the [LTO CLI Client](../../wallets/wallets/cli-client.md) on your system and import the account using the seed phrase. After that, you can set account data.

```bash
lto account seed <<< "$LTO_WALLET_SEED"
lto data set < nodeinfo.json
```
{% endtab %}

{% tab title="REST API" %}
Store the following JSON in a file on your server (eg `nodeinfo.json`). Change the `sender` and data values.

```json
{
  "sender": "NODE_ADDRESS",
  "data": [
   {
      "key": "node_name",
      "type": "string",
      "value": "My node name"
    },
    {
      "key": "node_description",
      "type": "string",
      "value": "Full description about your node"
    },
    {
      "key": "website",
      "type": "string",
      "value": "https://example.com"
    },
    {
      "key": "branding:logo_svg",
      "type": "string",
      "value": "https://example.com/logo.svg"
    },
    {
      "key": "social:telegram",
      "type": "string",
      "value": "@telegram_handle"
    },
    {
      "key": "social:twitter",
      "type": "string",
      "value": "@twitter_handle"
    },
    {
      "key": "payout_sharing",
      "type": "string",
      "value": "95%"
    },    
    {
      "key": "payout_schedule",
      "type": "string",
      "value": "Every week"
    }
  ]
}
```

To use the REST API of your node, you'll need your API Key (`LTO_API_KEY` environment variable).

```bash
curl -X POST http://localhost:6869/transactions/submit/data --data @- -H "Authorization: bearer API_KEY" < nodeinfo.json
```
{% endtab %}
{% endtabs %}

All properties, except `node_name` are optional. Other settings from the [community-proposed metadata standard](https://github.com/sbrekelmans/generator-info-standard), may also be used.
