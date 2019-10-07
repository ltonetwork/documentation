---
description: >-
  This page shows the 2 steps needed to get an LTO Network node up and running
  using IBM Cloud, specifically Kubernetes Services.
---

# Node: IBM Cloud

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

## **Step 1: Deploying your node on IBM Cloud using the browser interface**

_Make sure you have a “Pay-as-you-go” or better IBM Cloud account. It’s not possible to create a Kubernetes Service on IBM Cloud with a free account._

![IBM Cloud&#x200A;&#x2014;&#x200A;Main screen](https://cdn-images-1.medium.com/max/2400/1*QTd3ZGtj10NZjGuUIfEt1Q.png)

Shown above is the main screen of IBM Cloud after your initial login. As you can see at the time I had not upgraded to a pay-as-you-go subscription. Without such a subscription you can not create a “free” Kubernetes Service cluster, a requirement \(you have other more expensive paid alternatives\) for hosting a container on IBM Cloud.

Please make sure you have added a subscription to your plan. If you’re ready to continue click on **Create resource** in the top right corner.

![IBM Cloud&#x200A;&#x2014;&#x200A;Service Catalog](https://cdn-images-1.medium.com/max/2400/1*q5_pl5jqbmL11H9267E3Sg.png)

On the next screen user the filter to go through the catalog. We want to deploy a **Kubernetes Service**. Click the box to continue.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes Service overview](https://cdn-images-1.medium.com/max/2400/1*fT8iVLhrF0dHDong3FAI_g.png)

On the overview page you can click **Create** to create your Kubernetes Service cluster. If you do not have the option to click create you probably do not have the right subscription.

![IBM Cloud&#x200A;&#x2014;&#x200A;Create a new Kubernetes service cluster](https://cdn-images-1.medium.com/max/2400/1*Zswdcng4mTufy5MFIQyV6A.png)

In the next screen we have the opportunity to setup a simple test cluster or deploy an actual production-ready Kubernetes cluster. For the sake of this test I choose to go with the Free cluster. Select the **Free option**, change settings if you please and click **Create cluster.**

![IBM Cloud&#x200A;&#x2014;&#x200A;Alright, you requested a Kubernetes service cluster, let the waiting begin!](https://cdn-images-1.medium.com/max/2400/1*C8li3zO8q6hP0DvynMNaBQ.png)

You successfully requested a deployment of a Kubernetes Service cluster. Unfortunately this takes quite some time. It’s probably a good idea to grab a cup of coffee, tea or even better… beer!

_A Kubernetes cluster consists of multiple components. In the case of the free cluster a Master and a Worker will be deployed. The deployment of the Master will take approximately 30 minutes. The deploy of the Worker will take you an additional 5 minutes._

![IBM Cloud&#x200A;&#x2014;&#x200A;Yes! After 35 minutes we got our Kubernetes Service cluster running!](https://cdn-images-1.medium.com/max/2400/1*Wwg5X6aAX19PTlXQjjS31g.png)

With our cluster running it’s time to deploy our LTO Public Node application. Click **Kubernetes Dashboard** to open the Kubernetes dashboard main screen in a new tab.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes cluster dashboard](https://cdn-images-1.medium.com/max/2400/1*osBqFgmmQiwhPt3ruSUa8Q.png)

To deploy an application on the cluster click on **Create** in the top right corner.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes create an application](https://cdn-images-1.medium.com/max/2400/1*PSIES2JLGF-4wjuS2r8UIg.png)

Select the **Create an App** tab and fill in an **App name**, **Container image**\(important\). Now click on **Show Advanced Options**.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes new application configuration of memory and environment variables](https://cdn-images-1.medium.com/max/2400/1*Cqf4CUQstOsK-doBTe075g.png)

Please change the Memory requirement to **2048** \(2 GB\) and add your environment variables: LTO\_WALLET\_SEED, LTO\_WALLET\_PASSWORD and LTO\_NODE\_NAME.

{% hint style="info" %}
A future update might include the usage of secrets to secure our configuration.
{% endhint %}

Scroll down and click **Create**. Your LTO Public Node will be deployed on your freshly created Kubernetes Service cluster.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes dashboard showing your LTO Public Node successfully deployed](https://cdn-images-1.medium.com/max/2400/1*LV21sfR5syiF_EYTkEgppA.png)

Let’s do a final check and click on our pods name in the **Pods** section of this main screen.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes LTO Public Node pod configuration](https://cdn-images-1.medium.com/max/2400/1*b1IR9lzxxt0Yuieu_rawag.png)

This page shows the configuration of our running node. Please click on **Logs**in the top right corner to open a separate tab with our console logging.

![IBM Cloud&#x200A;&#x2014;&#x200A;Kubernetes LTO Public Node console logging](https://cdn-images-1.medium.com/max/2400/1*BpT97H4_KNNOdYYeY1VuOQ.png)

In the above example I’ve selected the auto-refresh option to make sure the console logging automatically refreshes every 5 seconds. Watch as the node downloads the blockchain and starts adding MicroBlocks.

That’s it. You’ve successfully mastered setting up a LTO Network Public Node on IBM Cloud using Kubernetes Services. Awesome!

## **Step 2: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

