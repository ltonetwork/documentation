---
description: >-
  This page shows the 2 steps needed to get an LTO Network node up and running
  using Microsoft Azure, specifically Azure Container Instances (Not to be
  confused with Azure Kubernetes Services).
---

# Node: Microsoft Azure

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

Below the 2-step setup procedure using the browser there is an optional procedure for using the command-line interface.

## **Step 1: Deploying your node on Azure using the browser interface**

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Main screen](https://cdn-images-1.medium.com/max/2400/1*TseFOVybCXuanMw82cV_oQ.png)

As mentioned before we’re configuring a Container instance using ACI. This is not to be confused with AKS, Azure Kubernetes Services. AKS can be used to setup a cluster for container orchestration. This is overkill for what we’re trying to achieve at this time.

Please type “container” \(without an s\) in the box and “Container instances” will appear. Click **Container instances**.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Base configuration](https://cdn-images-1.medium.com/max/2400/1*D3tcLPGLm_wvYcU--h2ygA.png)

First step is to configure some basic settings for your new container instance. You can not really go wrong here. The subscription entry displayed will display on your Azure subscription. You might need to create a Resource group using **create new**. Please name your container, select a Region where you want to host your container and make sure to enter the **image name**correctly. Before going to the next step we need to change 1 more thing.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Configure memory](https://cdn-images-1.medium.com/max/2400/1*M2xBigWwtitZ7ORkcXqFDg.png)

Let’s boost the memory a little from 1.5 GB default to 2 GB by clicking **Change size**. Now click on **Next: Networking** to go to the next step.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;No port / network changes required](https://cdn-images-1.medium.com/max/2400/1*5fxgkx0kyPn3vSq0RXi4fQ.png)

Wow, this one is easy. We don’t need to change anything here. Maybe you want to enter the DNS name label. It’s up to you. This is not a requirement. No ports need to be added at this point. You can leave it at default.

Please click **Next: Advanced** so we can start configuring our node.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Setup your environment variables](https://cdn-images-1.medium.com/max/2400/1*WbZFJ90fLdFPPSDQN6R51A.png)

Alright at this stage you want to configure your LTO\_WALLET\_SEED. This is an important step. This should be the seed of the wallet you use for staking. We configure the LTO\_WALLET\_PASSWORD to encrypt the seed. The LTO\_NODE\_NAME is the name your node will use to identify itself on the LTO Network.

{% hint style="info" %}
A future update might include the usage of secrets to secure our configuration.
{% endhint %}

When you’re done putting in these variables click on **Review + create**.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Validation passed&#x200A;&#x2014;&#x200A;Your screen might look slightly different](https://cdn-images-1.medium.com/max/2400/1*c7ogo7KfeBgsc1sGcpibjQ.png)

If validation is successful click **Create** to create your container and wait for the deployment to finish.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Deployment of Container instance is running](https://cdn-images-1.medium.com/max/2400/1*SJmz6YOL8Ltnkswq2kDHgg.png)

The deployment might take 1–2 minutes to complete.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;A successful deployment of your LTO Public Node container on Azure](https://cdn-images-1.medium.com/max/2400/1*idBIXedXKJ1B1SOuAZ0Jow.png)

Your LTO Public Node is now running on Azure using Azure Container Instances. Not convinced? Let’s double check :\) Please click on **Go to resource**.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Connected to the console](https://cdn-images-1.medium.com/max/2400/1*i09LFIhxy-L4IF_11KOj2A.png)

Let’s click on **Containers** under **Settings**. Now click on **Connect** and select a bash shell to **Connect**. A terminal will open. Activate the terminal by clicking on the black screen.

Using the cd \(change directory\) command we will browse to the /lto/log directory. In this directory you’ll find our logfile called: **lto.log**.

Start a tail like in the above screenshot and follow the progress as your node downloads the blockchain or maybe is already adding MicroBlocks.

![LTO Public Node on Azure&#x200A;&#x2014;&#x200A;Tailing your Node&#x2019;s logfile](https://cdn-images-1.medium.com/max/2400/1*wdZQgT9MFEBlCRneOQReWg.png)

That’s it. You’ve successfully mastered setting up a LTO Network public node on Microsoft Azure. Awesome!

## **Step 2: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

## **Optional: Deploying your node on Azure using the cmd-line interface**

Optionally you can start your container using the command-line interface. A big advantage of using the CMD is that you can automate everything.

Use the following commands to kick-off your LTO Network Public node using command line:

```text
CONTAINER_RESOURCE_GROUP="lto-network-rg"
CONTAINER_LOCATION="westeurope"
CONTAINER_NAME="lto-network-node"
CONTAINER_NODE_NAME="YOUR NAME"
CONTAINER_WALLET_SEED="SEED1 SEED2 SEED3"
CONTAINER_WALLET_PASSWORD="PASSWORD"

az group create --name $CONTAINER_RESOURCE_GROUP --location $CONTAINER_LOCATION

az container create --resource-group $CONTAINER_RESOURCE_GROUP --name $CONTAINER_NAME \
 --image legalthings/public-node \
 --cpu 1 \
 --memory 2 \
 --os-type linux \
 --ports 80 \
 --restart-policy Always \
 --environment-variables "LTO_NODE_NAME"="$CONTAINER_NODE_NAME" \
 --secure-environment-variables "LTO_WALLET_SEED"="$CONTAINER_WALLET_SEED" "LTO_WALLET_PASSWORD"="$CONTAINER_WALLET_PASSWORD"
```

With just these two commands you have successfully launched your LTO Network Public node container application. Using the following command you can view your nodes progress:

```text
az container logs --resource-group $CONTAINER_RESOURCE_GROUP --name $CONTAINER_NAME
```

You can double check your environment variable configuration using the following commands:

```text
az container exec --resource-group $CONTAINER_RESOURCE_GROUP --name $CONTAINER_NAME --exec-command "/bin/bash"
$ echo $LTO_WALLET_SEED 
$ echo $LTO_WALLET_PASSWORD 
```

