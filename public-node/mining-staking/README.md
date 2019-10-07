---
description: You can participate in network consensus and earn LTO.
---

# Node Setup & Staking

Layer 1 of the LTO Network's [hybrid architecture](../../company-area/about-tech/) is permissionless public blockchain. This means you can set up a node and become a validator, helping decentralize and secure the network. If you are not tech-savvy, you can just lease your mainnet LTO to other nodes and share network rewards with them. 

{% hint style="info" %}
Network mining is accessible only for [mainnet LTO](../../company-area/token/). You can swap ERC-20 to mainnet via the [Bridge Troll](../../company-area/token/bridge-troll.md). You can **combine it with** [**social mining**](../social-mining/) and earn more rewards.
{% endhint %}

Here are a few key points for the miners:

* **There is no network inflation, mining rewards are the transactions fees paid by network users**. They can be coming from normal transfers, but around 99% of them are anchoring transactions resulting from clients and integrators using LTO Network engines. Check the [calculator](https://lto-lease.com/tools/roi).
* Layer 1 public blockchain based on the NG protocol. This means the rewards are 40%-60% split between the miners of micro blocks and a key block. Don't be confused if you see your rewards being either 0.10 LTO or 0.15 LTO, NG is the reason for that.
* Up until Q3/Q4 2019, the reward probability is based on Fair Leased Proof of Stake. This means that the size of your stake is the major determining factor. This is called sybil resistance.
* LPoS somewhat leads to centralization of a network, which is why we in Q3/4 we are switching to our own development: [Leased Proof of Importance](../../company-area/token/lpoi.md), which accounts for actual network usage.
* The Bridge Troll fee from ERC-20 side to Mainnet, which is 40 LTO, gets split as 16 LTO - 24 LTO. This adds some more rewards for the nodes.

You can read more about the structure in the Developer area.

{% hint style="info" %}
Node rewards are [transactions fees ](http://dev.pywaves.org/LTO/generators/)of the network and correspond with the ratio of your staked amount over the total staked amount in the network. While 1,000 LTO is the requirement, it will most likely not yield frequent rewards. You can find the calculator [here](https://lto-lease.com/tools/roi).
{% endhint %}

## Run a node and stake

The node setup fits in one tweet, it's super simple to set up!

`1) git clone https://github.com/ltonetwork/lto-public-node    
2) vim docker-compose.yml   
3) docker-compose up`

Below you'll find the list to the installation guides for setting up a node on various platform. Make sure to check out the [Prepare: Setup your wallet](prepare-setup-your-wallet.md) page before jumping into the installation of your node.

* [Ubuntu Linux with container](node-ubuntu-linux-with-container.md)
  * [Access to your  node from the outside world using an Nginx reverse proxy](optional-nginx-reverse-proxy.md)
* [Windows 10 with container](node-windows-with-container.md)
* [Raspberry Pi](node-raspberry-pi-expert.md)
  * [Explained also with a few pictures](https://medium.com/@nachomartinez99/how-to-install-a-lto-node-on-a-raspberry-pi-2-20c34a01167d) \(External website!\)
* [OKD \(OpenShift Origin, Kubernetes\)](node-okd-openshift.md)
* Public Cloud:
  * [Alibaba Cloud \(Elastic Container Instances\)](node-alibaba-cloud.md)
  * [AWS Elastic Beanstalk ](node-aws-elastic-beanstalk.md)
  * [Azure \(Azure Container Instances\)](node-microsoft-azure.md)
  * [Google Cloud \(Kubernetes Service\)](node-google-cloud.md)
  * [IBM Cloud \(Kubernetes Service\)](node-ibm-cloud.md)

## Lease LTO and Split Rewards

With the traditional Proof of Stake algorithm it is important to have a big wallet, only people with a certain amount of tokens can run a node and forge blocks on the blockchain. With Leased Proof of Stake, token holders can lease their wallet balance to someone who is running a node, without having to run anything themselves. Most node operators will share a percentage of their node forging rewards with their leasers. The higher the leased amount of a node is, the more chance it has in being eligible to forge the new block. If the block has transactions, the forging node will earn the fees from those transactions.

When leasing, you are not transferring your tokens to someone else. Your tokens stay in your wallet. So you cannot lose your LTO this way, it's SAFU.

{% hint style="info" %}
**Every node is like its own miniature community, it's awesome :\)** 

* Create a [mainnet wallet](https://wallet.lto.network/start) and save your seed phrase
* Swap ERC-20 LTO to mainnet LTO - be careful of the [Bridge Troll](../../company-area/token/bridge-troll.md)
* Read about different leasing communities in the table below - calculator [here](https://lto-lease.com/tools/roi)
* Press one button **lease**... done!
* If you hold mainnet LTO, you can **combine network mining with** [**social mining**](../social-mining/) and earn even more rewards by helping us out with different tasks.
{% endhint %}

| **Address** | Telegram Contact | Website / Community | Description |
| :--- | :--- | :--- | :--- |
| [x9oM](https://explorer.lto.network/address/3JnZLvmVBXsc2XMug4e6yjyvPehr1Fjx9oM) | @N0rthstar | [LowSea Leasing](https://t.me/joinchat/ALw70hNg64IIUx2vd3qU8g) | Sander is educating the Dutch community, helps with node set up and external relations. |
| [7JJ5](https://explorer.lto.network/address/3JqGGBMvkMtQQqNhGVD6knEzhncb55Y7JJ5) | @jayjay | [Liquid Leasing](https://t.me/liquidleasingnetwork) | Jayjay [built](../network-overview-tools.md) a payout script, helps out with Dutch communities and tech implementations. |
| [imvJ](https://explorer.lto.network/address/3Jw2JvjSYcYyKFExHaYFGKyZnHDhJ9TimvJ) | @PatrickS79 | [Main LTO chat](https://t.me/LTOnetwork) | Patrick is making weekly Network Activity Reports. |
| [zwFiv](https://explorer.lto.network/address/3JnN8psLjuEyiPbH2bYcEFKUFpcamxzwFiv) | @LTO\_node | [ltonode.com](https://ltonode.com) | Erwin [built](../network-overview-tools.md) network stats overview, staking calculator, and so on. |
| [UVBp](https://explorer.lto.network/address/3Jhkp3Xtg2wyT6NoEtJB2VQPAHiYuqYUVBp) | @aorfevrebr | [LTO Lease](https://t.me/ltolease) | Alex is helping with [bot](../network-overview-tools.md) technical implementations. |
| [Eyu9](https://explorer.lto.network/address/3Jq3F3njrrR1ZvM3JhwLX2Sh56LQDtuEyu9) | @Cryptocapitalist | [Smart Workflow](https://t.me/Smart_Workflow_Node_for_LTO) | Joel, Ignacio, and others are managing the biggest Spanish crypto community. They have also [built](../network-overview-tools.md) a node utility bot. |
| [t9b9](https://explorer.lto.network/address/3JeUGgoCUy5wXpNKHqhaLpvGZrshtvwt9b9) | @G1zm0 | [dev.pywaves.org/LTO](http://dev.pywaves.org/LTO/generators/) | Rob [built](../network-overview-tools.md) a network overview board, built a payout script, and is helping with tech upgrades. |
| [CK2P](https://explorer.lto.network/address/3Jn6jpPBVmi1RLRpUtQGKVibNZpeTRACK2P) | @PitbullCH | [kruptosnomisma.com](http://www.kruptosnomisma.com/) | Kruptos Nomisma are running nodes for different projects. |
| [8kKU](https://explorer.lto.network/address/3JexCgRXGFUiuNoJTkkWucSumteRWdb8kKU) | @fexraTRTL | [lto.services](https://lto.services) | Fexra [built](../network-overview-tools.md) complex analytical tool with more stats, and a community explorer. |
| [m23M](https://explorer.lto.network/address/3JyjGvJcGRkBLF3BNocfR7v63txGGt7m23M) | @Quantumpire | [quantumfart.threadless.com](http://quantumfart.threadless.com/) | Xander is making meme merch with fun LTO pictures. |
| [vvbB](https://explorer.lto.network/address/3JtBYdoHJoQbgf8hYvEz1pyp7jj9URWvvbB) | @JellevanderSteen  | [bit.ly/2FuD1b0](http://bit.ly/2FuD1b0) | Jelle and Marco are educating Dutch community members. |
| [FiEX](https://explorer.lto.network/address/3JmcAJMQhdLKj296xoDkng9r1McCmBSFiEX) | @NUT65 | [ltoleasing.com](http://www.ltoleasing.com) | Arie is long-term LTO supporter from the Dutch community. |
| [Kj7k](https://explorer.lto.network/address/3JfWZRLvCSjMGNMNtNUWPa6BiQXiJgvKj7k) | @ICODOGCHRIS | [ICO DOG](https://t.me/ICO_DOG_POOL) | Chris, Gio, and others help with community management. |
| [FA89](https://explorer.lto.network/addresses/3JgAqbUkPnPVqAtyiFVe2HD1CSuXektFA89) | @antonioee | [LTO lottery node](https://telegra.ph/LTO-lottery-node-05-04) | Anton is managing a shady crypto OTC market and helps with the Russian community. |
| [9hKD](https://explorer.lto.network/addresses/3Jj5Ws9ZdP1xWKgR2S2MQGJN7RJrpb99hKD) | @WvonR | [lto-node.com](https://lto-node.com/) | Willem is an active Dutch community member. |
| [MU7U](https://explorer.lto.network/addresses/3Jki2nTc2KZ9ww3jyvrQLnhGLBLwKneMU7U) | @theeaglefliesalone | [LTO News](https://twitter.com/ltonews  ) | Sadık is one of the our supporters from Turkish community. |
| [fVnm](https://explorer.lto.network/addresses/3JntAfEPcUSEaonDZsFq5RskxKNHd28fVnm) | @D00hanPijo | [LTO VIP Node](http://lto-vip-node.trade/) | Professional node, only for limited number of leasings. |
| ... | ... | .... | Your node, your community! |



