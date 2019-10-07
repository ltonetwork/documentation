---
description: >-
  This page shows the 2 steps needed to get an LTO Network node up and running
  using Google Cloud, specifically using the Kubernetes Engine called Google
  Cloud Engine.
---

# Node: Google Cloud

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

## Step 1: **Deploying your node on Google Cloud using the browser interface**

Setting up a LTO Public Node on Google Cloud Engine is very easy. Google automates almost everything for you.

It’s quite a heavy setup for a LTO Public Node though because of the required Kubernetes cluster. In my case Google spun up a Kubernetes Cluster when running using around 12 GB of memory. Because of this requirement running a node on Google Cloud Engine will probably be quite “expensive” \(You determine the value and define what expensive means :\) \).

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Main screen](https://cdn-images-1.medium.com/max/2400/1*IPq70LODxJghc2363rw5fg.png)

Yes, we are in the main screen. Let’s jump in immediately and click **Kubernetes Engine** -&gt; **Clusters**.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Deploy container](https://cdn-images-1.medium.com/max/2400/1*LDcn9MxG-5P7CzmR03B7_Q.png)

Before you reach the above screen you might have to enable billing. This in turn will enable the GCE — Google Cloud Engine API. Please wait 1–2 minutes before continuing.

Having enabled the API when can start with our deployment. Click **Deploy container**.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Creating a container deployment step 1](https://cdn-images-1.medium.com/max/2400/1*pbScQ3JmVVsOFgITi55ROg.png)

We immediately jump into configuring our container. This does not mean we do not need networking, security, a Kubernetes cluster, etc. It simply means that Google can take care off a lot of these steps.

_On the right side under **Environment variables** you find an important node regarding access secrets. It’s definitely best practice to use secrets to store your valuable data like password\(s\) and also your seed. Using secrets for your environment variables makes sure they do not show up in plain text. This guide does **not** use access secrets. Let me know if you want me to add this!_

Let’s continue creating our deployment:

* Select **Existing container image**
* **Image Path**: legalthings/public-node
* Set your environment variables: LTO\_WALLET\_SEED, LTO\_WALLET\_PASSWORD and optionally your LTO\_NODE\_NAME.

{% hint style="info" %}
A future update might include the usage of secrets to secure our configuration.
{% endhint %}

Click **Continue**.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Creating a container deployment step 2](https://cdn-images-1.medium.com/max/2400/1*_zSVFLK1osU80RYdInQHPQ.png)

Step 2 of the deployment is nothing more than naming your application. Please choose an **Application name**.

Now it’s time to create your deployment!

![Google Cloud Platform&#x200A;&#x2014;&#x200A;The Deployment of the entire process](https://cdn-images-1.medium.com/max/2400/1*pp0FPmyI0YOw76nfTAR5Lg.png)

Google will take care of the deployment for you. This will take around 5 minutes. Google will start by deploying a Kubernetes Cluster. Second it will create your deployment, just like you configured and then this will spin up your pod \(containing your running LTO Public node container\).

Please wait for the process to finish.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Deployment overview screen](https://cdn-images-1.medium.com/max/2400/1*0MhVJeAz7DHpIrcv-R31pA.png)

Something happened, our application got deployed on our freshly created Kubernetes cluster. We are not happy though! A thing called an auto-scaler is active. We want to disable this as it makes our application run multiple times in parallel.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Step 1 to disable auto-scale](https://cdn-images-1.medium.com/max/2400/1*6YgVeMiDUfNR7GYHtnZCeA.png)

Click **Actions** → **Auto-scale** to open auto-scale settings.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Step 2 to disable auto-scale](https://cdn-images-1.medium.com/max/2400/1*KuW5dgFZ6W_wGUyAl-sqBw.png)

Click on **Disable Autoscaler** to disable the auto-scale feature.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;Back in the deployment overview, now with just 1 managed pod running!](https://cdn-images-1.medium.com/max/2400/1*ofQck-GDLmkfkudZ0Ztbkw.png)

Back in our overview we see we only have 1 managed pod. This is what we wanted to achieve. Now let’s see how our LTO Public Node is doing by diving in the logs.

Click **Container logs** in the middle of the screen to jump to the logging screen.

![Google Cloud Platform&#x200A;&#x2014;&#x200A;See our node in action](https://cdn-images-1.medium.com/max/2400/1*uUz8CJJCM3AZeyvnrQJk8Q.png)

In the logging screen you have the ability to click the **Play** icon to start streaming the console log of the container. You can see the blockchain being downloaded. After the blockchain has been successfully downloaded the node should start appending MicroBlocks like in the above screenshot.

That’s it. You’ve successfully mastered setting up a LTO Network public node on Google Cloud. Awesome!

## **Step 2: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

