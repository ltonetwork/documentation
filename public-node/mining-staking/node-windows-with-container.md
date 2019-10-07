---
description: >-
  This page explains in just a few steps how to get your node up and running
  using Windows 10 and a Docker container.
---

# Node: Windows with Container

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

## **Step 1: Preparing the setup of the node**

Get Docker installed. Go to [https://docs.docker.com/docker-for-windows/](https://docs.docker.com/docker-for-windows/) and follow the steps to install it. This is a simple download and execute, but you might need to tweak some settings on your computer. The install will ask to restart Windows in order to activate Hyper-V.

**Docker minimum requirements:** 

* Windows 10 64 bit: Pro, Enterprise or Education \(1607 Anniversary Update, Build 14393 or later\)
* Virtualization is enabled in BIOS. Typically, virtualization is enabled by default. This is different from having Hyper-V enabled. For more detail see Virtualization must be enabled in Troubleshooting.
* CPU SLAT-capable feature
* At least 4GB of RAM

{% hint style="info" %}
Your computer’s CPU needs to have virtualization enabled. You can check if your CPU can handle this by installing software like ‘Speccy’ and find Youtube videos on how to activate virtualization in your BIOS if you need to. If your system does not support Hyper-V and CPU virtualization to run Docker for Windows as an alternative you can install ‘Docker Toolbox’, which uses Oracle VirtualBox instead of Hyper-V.
{% endhint %}

Next up is to start Docker for Windows to ensure it is installed correctly.

Once Docker is installed, create a simple folder on your computer \(example ‘C:\LTO\’\) and download the necessary file\(s\) from LTO github to them

* Go to Link: [https://github.com/ltonetwork/lto-public-node](https://github.com/ltonetwork/lto-public-node)
* Download the file: **docker-compose.yml** to the directory you just created

Open the **docker-compose.yml** file in software like Visual Studio Code \([https://code.visualstudio.com/](https://code.visualstudio.com/)\), it may ask you to install the Docker extension once you open the yml file. Other editors can also work, but things like Wordpad and Notepad will garble up your content. Notepad++ seems to handle it well too though.

Edit the following values:

```text
- LTO_WALLET_SEED=<Input the seed of your wallet>
```

This means: look for ‘LTO\_WALLET\_SEED=’ and put the seed words \(including the spaces\) directly after ‘=’. Delete all the default text between &lt; and &gt; including the &lt; and &gt;.

```text
- LTO_WALLET_SEED_BASE58=<Input the seed of your wallet but then base58 encoded>
```

Choose 1 of the above. You do not need to configure both.

```text
- LTO_WALLET_PASSWORD=<You choose a password>
```

This password is used to encrypt your seed on the disk of your node. It can be the same password as your wallet password when you created your LTO\_wallet, but naturally this is not recommended.

```text
- LTO_API_KEY=<You choose an API password>
```

Configure a your password you want to use to access the admin options of your API.

{% hint style="info" %}
The total yml is about 17 lines. If the editor shows you much more, this text is all formatting, which is bad as the node will not properly read the contents and run! If this happens, click on the yml file on Github itself and copy paste the 17 \(more or less\) lines \(including all the spaces\) and overwrite the contents of your editor.
{% endhint %}

Save the docker-compose.yml file in the directory you created. For example: C:\LTO\.

## Step 2: Startup our container on our Windows system

Open a command prompt \(or another terminal like PowerShell\). Go to the directory where the yml file is located and type:

```text
c:\> cd LTO
c:\LTO> docker-compose up
```

Docker will now start downloading the node image and get up-to-date on the transactions. Once transaction blocks are updated it will start mining blocks and recording transactions \(The output will display MicroBlocks being appended\).

{% hint style="info" %}
You can type ‘**docker-compose up -d**’ to setup and run the node in the background. This will allow you to close the terminal as well, while the node keeps running.
{% endhint %}

Some Docker Commands that could proof handy:

1. ‘docker images’: shows all images
2. ‘docker ps -a’: shows all \(running\) containers
3. ‘docker start/stop public-node’: start node or stops a running node
4. ‘docker logs -f public-node’: Check on the running node and see the transactions.
5. ‘docker-compose down’: delete the container
6. ‘docker-compose pull’: pull the latest image into your repository. Handy if you ran testnet and the yml keeps launching the testnet node-image.

## **Step 3: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

## Additional information

### **Docker compose seemed to work but my node is not mining. What should I do?**

* Check if your yml file has the right SEED words filled in. Make certain you did not accidentally leave &lt; and &gt; after the ‘LTO\_WALLET\_SEED=’ notation.
* You can check the LTO wallet address of your public node by going to your webbrowser and typing in: http://localhost:6869 \(if you changed the port in the yml file, make sure you fill in the correct port here and if localhost does not work check below as the rest-API is not activated by default on mainnet\).
  * Click on: addresses -&gt; \|GET\| /addresses -&gt; Try it out! The Response Body should show you the address for your own wallet.

### **Some suggestions made by the LTO team**

Those that run a VPS, please add the following lines:

* Under port: - 6868:6868
* Under environment: - LTO\_DECLARED\_ADDRESS=**yourIP**:6868
* Under environment: - LTO\_ENABLE\_REST\_API=true 
  * this will allow you to approach your node through the API webinterface
* Additional command lines can be found on the LTO-github:
  * [https://github.com/legalthings/docker-public-node](https://github.com/legalthings/docker-public-node)

### **An example of the entire yml file with the suggestions made above active**

```text
version: '3'
services:
 public-node:
  container_name: public-node
  image: legalthings/public-node
  ports:
   - 6869:6869
   - 6868:6868
  environment:
   - LTO_WALLET_SEED=all your seedwords input here
   - LTO_WALLET_PASSWORD=the password you created
   - LTO_API_KEY=the API key you created
   - LTO_ENABLE_REST_API=true
   - LTO_DECLARED_ADDRESS=YourIP:6868
```

