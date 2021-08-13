---
description: Run and manage your own node.
---

# Node management

## Basic considerations

There are some initial considerations you should clear before deciding on running and managing your own node. Since you have come this far in the tutorials you have probably already consulted the technical requirements for running your own node:

{% page-ref page="../../public-node/installation-guide/requirements.md" %}

Next up are the economic aspects. The minimum amount of tokens needed is 1000 for your node to participate in the network. However, with 1000 LTO your node probably will not be generating enough LTO tokens to cover your server costs. You should consult the next paragraph _Probability to generate blocks_ to calculate the necessary amount for you. At present this number is anywhere between 70k - 100k, depending on your provider. If you have below that you might consider staking on one of the public community nodes instead.

Lastly, you might want to become one of the public community nodes yourself. If so the last paragraph of this page, _Paying out rewards_ is definitely worth a read. Also, the tutorial on how to become a community node is a must-read as well:

{% page-ref page="public-community-nodes.md" %}

If not, there is no real necessity for you to dive deep into node reward payments because all of the rewards will be consolidated and staked on your node automatically.

## Probability to generate blocks

By running your own node you participate in the network and get a chance to generate new blocks. These blocks will provide your node with the node rewards depending on the number of transactions in the block. The more transactions are collected in your node's generated block, the higher the node reward will be. The staking consensus is regarded in more detail in the [LTO Whitepaper](%20https://ltonetwork.com/documents/LTO%20Network%20-%20Technical%20Paper.pdf), see section 15 and especially the subsections 15.1 to 15.4 to have a thorough understanding of LTO's consensus.

How many blocks your node will generate depends on the funds staked on the node. Generally speaking, the higher the stake is the more blocks you will generate. The community set up an exemplary [Community model](https://docs.google.com/spreadsheets/u/0/d/1KcqI0Uay0ogJL8TILqKjESjiz0bwMrYeMz8k5TCUbHA/htmlview) to visualize the probability of an LTO stake generating new blocks. _There you can see how high the probability is to generate a block after a certain amount of time has passed._

{% hint style="info" %}
A probability to generate a new block in 24 hours of 90% means that 9 out of 10 days you should generate a block daily. While this may manifest as a block each day, it can also manifest as two blocks every second day and none in between.
{% endhint %}

If after initially starting your node you find it does not generate blocks as fast as you think it should don't be discouraged. The initial node setup takes time until your node is fully synchronized with the network and the blockchain is up-to-date on your machine. Check your node logs for errors and check the community overviews \(e.g., [lto nodes ](https://www.ltonod.es/)or [lto tools](https://lto.tools/nodes/)\). 

{% hint style="info" %}
To check your node's logs run `docker logs -f public-node`. This way you can detect grave error messages, for example, network problems.
{% endhint %}

To have a rough estimate of how long you will have to wait before generating a block you can use the following approximation:

$$
((1 - (s_1 / S ))^d)^h = P
$$

with

* $$S$$ being the total amount of staked LTO on the network
* $$s_1$$being the amount of LTO staked on your node
* $$d$$ being the number of blocks forged per hour
* $$h$$ being the number of hours your node will be running without generating a block.

Play around with the $$h$$ parameter to determine when the resulting probability $$P$$ goes close to zero, as that is the amount of time you will have to wait on average before generating a block. If you want the _percentage_ simply multiply $$P$$ by 100.

{% hint style="warning" %}
This formula is an oversimplified version of the actual probability from the [LTO Whitepaper](%20https://ltonetwork.com/documents/LTO%20Network%20-%20Technical%20Paper.pdf). 
{% endhint %}

## Paying out rewards

If you have several wallets leasing to your node or simply plan to pay out your node rewards into your own wallet on a regular basis, you will need to look for payout scripts. Luckily the community has got you covered on this one:

{% embed url="https://github.com/jayjaynl/LTO\_LPoSDistributor" %}

The LPoSDistributor provides scripts to handle the day-to-day node payments. This tutorial is simply a boiled-down explanation from the project's README, please refer to it for a more in-depth view. The project is subdivided into three parts:

* the collection logic \(appng.js or start\_collector.sh\)
* the validation logic \(checkPaymentsFile.js\)
* the payment logic \(massPayment.js or masstx.js\)

{% hint style="warning" %}
If you encounter problems running start\_collector.sh on UNIX machines it may be due to the DOS line endings. Run a `sed -i -e 's/\r$//' start_collector.sh` to fix that.
{% endhint %}

### Collection logic

First, you'll need to configure the file batchinfo.json once so that the script knows the starting point of all the computations. All future updates will do this automatically, so you only have to configure the first run:

```javascript
EDIT file batchinfo.json with vim or nano;

{
    "batchdata": {
        "paymentid": "1",				<== Leave as is
        "paystartblock": "1044012",			<== Put here same value as 'scanstartblock'. It's when payouts should start
        "paystopblock": "1050000",			<== Put here a value when payouts should stop (i.e. paystartblock+5000)
							    It doesn't really matter, as long as it is higher than paystartblock.
							    It only counts for the first run, and if no blocks were forged yet, that
							    is no problem. Follow up session results are just queued up in line :-))
        "scanstartblock": "1044012"			<== Put here the blockheight of the first ACTIVE lease
    }
}
```

Next up, is the computation of the collected blocks. This happens in the file appng.js, and you'll need to perform the following changes:

```javascript
const myleasewallet = '<your node wallet>';		<== Put here the address of the wallet that your node uses
const myquerynode = "http://localhost:6869";		<== The node and API port that you use (defaults to localhost)
const feedistributionpercentage = 90;			<== How many % do you want to share with your leasers (defaults to 90%)
const blockwindowsize = 10000;				<== How many blocks to process for every subsequent paymentcycle.

var nofeearray = [ ];					<== Put here wallet addresses that you want to exclude from payments,
							    Default empty, so everyone get's payouts
```

With this, you have a configured collection and reward computation logic. You can run it with `node appng.js` or `./start_collector.sh`.

{% hint style="warning" %}
Make sure start collector.sh is actually executable by running `chmod u+x start_collector.sh`. 
{% endhint %}

You can also automate this step by including start\_collector.sh into your crontab in your /etc/crontab file:

```javascript
00 01 * * * root cd /home/myuser/LTO_LPoSDistributor/ && ./start_collector.sh
```

### Validation logic

To validate the computations you can use the file checkPaymentsFile.js. It will list the next payments coming from your node and provide you with the opportunity to do some sanity checking before you actually start the payments in the next step. You can start the validation with '`node checkPaymentsFile.js`' but before you need to edit it:

```javascript
var config = {
    <SNIP>,
    node: 'http://localhost:6869',			<== Change this value to your blockchain node/API port (defaults to localhost)
    <SNIP>
};
```

### Payment logic

Finally, you should be able to pay out the rewards you computed and validated in the steps before. First, edit the files **massPayment.js** and **masstx.js**:

```javascript
var config = {
    <SNIP>,
    node: 'http://localhost:6869',			<== Change this value to your blockchain node/API port (defaults to localhost)
    apiKey: 'your api key'				<== Put here the API key of your lto node
};
```

Usually, it makes the most sense to use **masstx.js** to pay out your rewards, since your fees will be minimized. Run '`node masstx.js`' to finalize the payment and payout your node's rewards to your stakers.

{% hint style="warning" %}
For security reasons, remove 'rwx' world rights from massPayment.js and masstx.js: `chmod o-rwx massPayment.js`
{% endhint %}

