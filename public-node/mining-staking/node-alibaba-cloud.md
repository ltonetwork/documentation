---
description: >-
  This page shows the 2 steps ( >:) ) needed to get an LTO Network node up and
  running using Alibaba Cloud, specifically Elastic Container Instances.
---

# Node: Alibaba Cloud

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

Be prepared for some **tough** configuration. Configuring a container instance on Alibaba is not at all straightforward and a lot of manual networking and security related steps are involved!

## **Step 1: Deploying your node on Alibaba Cloud using the browser interface**

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Main screen](https://cdn-images-1.medium.com/max/2400/1*lBOzXNpNE7aZTIrWTNMnNA.png)

Setting up an LTO Public Node on Alibaba cloud is \_not\_ possible using a free account. There are paid services involved like a NAT Gateway and Elastic IP. Make sure you have entered your payment parameters before continuing.

Alright, let’s go!

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Product overview](https://cdn-images-1.medium.com/max/2400/1*ccZVFxuIAp0bCB2EGgd6YA.png)

Alibaba Cloud offers multiple Container offerings. The Container Service gives you the option of configuring a Kubernetes cluster or Swarm cluster. We do not want this. We just want to deploy **1** container.

We’ll go with the **Elastic Container Instance** option.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Elastic Container Instance main screen](https://cdn-images-1.medium.com/max/2400/1*ufJOfrKC0i665UMzlnqSZQ.png)

On the Elastic Container Instance product page click **Create Elastic Container Group**.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;ECI main configuration page](https://cdn-images-1.medium.com/max/2400/1*TUBvb-a14pZdin2kt9GlGg.png)

We’ll come back to this page a few times. Unfortunately we have to setup various network and security related stuff before being able to deploy our container.

All links you’ll click will open in a new Tab which makes it easy to jump back.

**Important:** Make sure you configure the resources in the region you’re actually using for your container deployment. In the above screenshot you see the US West 1 and then Zone B. All my configuration is done in Zone A! I will have to switch to be able to select my netwerk components, for example the vSwitch.

The configuration container configuration page asks us to select a VPC. We do not have one yet so let’s create one by clicking **Go to Console and Create**.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;VPC, Virtual Private Cluster configuration](https://cdn-images-1.medium.com/max/2400/1*E94ExmWfJVf6s5MhoCWdkg.png)

In the next screen click on **Create VPC**. In the screen appearing configure a VPC and a vSwitch. As you can see in the screenshot above. This is were you configure the Zone.

Next up is configuring the NAT Gateway. Click on the **NAT Gateways** option in the menu on the left.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;NAT Gateways overview](https://cdn-images-1.medium.com/max/2400/1*YHCstGaPRKeSfFUuce1DqA.png)

No NAT Gateway available yet. In the screen appearing click on **Create NAT Gateway** to create one.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;NAT Gateway&#x200A;&#x2014;&#x200A;This is a paid service.](https://cdn-images-1.medium.com/max/2400/1*2zW2s6SVcL_kAD5S-jfKhA.png)

Let’s go with the defaults. A Small Specification should be enough. Continue.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Finish your order](https://cdn-images-1.medium.com/max/2400/1*FQRg8DXzJGF6OkPYM9q0Uw.png)

With your order complete you can refresh your NAT Gateways overview page. You will see your NAT Gateway appearing. Let’s go to the next option to configure. The EIP.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;EIP configuration page](https://cdn-images-1.medium.com/max/2400/1*nlirvcvYFlNue1VRJLrgnw.png)

The EIP is our Elastic IP Address. Click on **Elastic IP Addresses** in the menu on the left. Click **Create EIP** in the screen appearing to create our Elastic IP address.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Another paid service :\)](https://cdn-images-1.medium.com/max/2400/1*nmEfeGlN_ReZhlkWo0PfkA.png)

Not sure about the max bandwidth needed. I went with 50 Mbps in this example. Make sure you have the region configured as the region were you’ll be deploying your container. Click on **Buy Now** and finish the rest of the transaction.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;EIP Available](https://cdn-images-1.medium.com/max/2400/1*2GHPmGN8ysj-Pe8UaFpCxg.png)

In the above screenshot you can see that we have our freshly ordered EIP available. It’s time to bind the EIP to our NAT Gateway. Click **Bind** on the far right.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Bind EIP to our NAT Gateway](https://cdn-images-1.medium.com/max/2400/1*Zt5KoL7BxbbrOXIowx2FSQ.png)

We want to bind the EIP to our NAT Gateway. Make sure to select **NAT Gateway Instance** in the Instance Type field. Now select your **NAT Gateway Instance** and click **OK.**

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;EIP bind to NAT Gateway &#x2192; Allocated](https://cdn-images-1.medium.com/max/2400/1*NS1NuftaVqvUYMVaSf2lgA.png)

Good job! You’ll see the status changing from Binding to Allocated when successful. With this done we can go back to our NAT Gateway and configure Source NAT.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;NAT Gateways overview](https://cdn-images-1.medium.com/max/2400/1*502ZW3o9Y0O8uOyyLkfNww.png)

Click on **Configure SNAT** on the far right to configure Source NAT.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Add a SNAT entry to our table](https://cdn-images-1.medium.com/max/2400/1*VqP3BoDCjDwgwP8fxOFY7w.png)

To continue click on **Create SNAT Entry** a new screen will appear.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Create SNAT Entry page](https://cdn-images-1.medium.com/max/2400/1*JgDdNaFqS246-EDQWcRnNg.png)

Select our vSwitch and select our EIP. Your IP address will differ from the above screenshot. Now click **OK** to continue.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;We&#x2019;re back in the ECI configuration page](https://cdn-images-1.medium.com/max/2400/1*7fulmEwc2I6JSMdAzxV21A.png)

OK, we’re back in the ECI configuration page. As you can see we switched our zone to Zone A as I configured my vSwitch in Zone A. I’ve selected my VPC and my vSwitch.

Next up is the configuration of a Security Group. We’ll do this by clicking on **Create Security Group**.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Security Groups overview](https://cdn-images-1.medium.com/max/2400/1*NBiSWu8-nZNo33nD3QYgnA.png)

Click **Create Security Group** to continue.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Rules!](https://cdn-images-1.medium.com/max/2400/1*IGDikoR6vGapAw_HX2Z2LA.png)

We do not really want rules but still, let’s click on **Create Rules Now**. Accept the proposed rules.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Security group ingress overview](https://cdn-images-1.medium.com/max/2400/1*i5urc99VHdpSyN_AiKGtlQ.png)

Go to the ingress rules tab of our security group. Select all the rules there and remove them. We do not need ingress rules to make our LTO Public Node work.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Back to the ECI configuration](https://cdn-images-1.medium.com/max/2400/1*wXNF8hJf3fTiUcXuM6sKfw.png)

With our Security Group created and configured we can finally continue with the configuration of our Elastic Container Instance. Click on **Select Security Group** and select our freshly created security group from the list. Click **Select**.

Now scroll down to the Container Group Name for the next step.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Finally, some container configuration](https://cdn-images-1.medium.com/max/2400/1*RPalXyW13KgyFzui1F2m2Q.png)

We’re almost there. It’s time todo some container configuration.

Please enter a:

* Container Group Name: you choose!
* Container Name: you choose!

Fill in the Docker Image name for the public node and fill in “latest” in the version field.

Select 1 vCPU and the 2GiB memory option will appear. We do not need 4 GiB.

Finally enter the environment variables. Unlike in the above screenshot we need to configure 3 parameters:

* LTO\_WALLET\_SEED: The seed of the wallet you use for staking
* LTO\_WALLET\_PASSWORD: You choose a password
* LTO\_NODE\_NAME: Optional but can be a good idea :\)

{% hint style="info" %}
A future update might include the usage of secrets to secure our configuration.
{% endhint %}

Let’s **Create** the container.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;ECI overview page](https://cdn-images-1.medium.com/max/2400/1*JV8AHUw9tMySk0I9xWFowQ.png)

A small summary before kicking off the creation process. If everything looks good to you click on **Create ECI** to activate the container.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;ECI instance successfully activated](https://cdn-images-1.medium.com/max/2400/1*mDCQchWlsQV5SaqOr_-49Q.png)

Yes, we did it. We created our Elastic Container Instance on Alibaba Cloud! Click **Console** to see our container in action.

![Alibaba Cloud&#x200A;&#x2014;&#x200A;LTO node&#x200A;&#x2014;&#x200A;Looking good! blockchain downloaded, appending MicroBlocks!](https://cdn-images-1.medium.com/max/2400/1*N-Xcl7YOptOukswRLwnX_g.png)

In the above screenshot i have selected our freshly created container instance and the **Logs** tab. I’ve selected the **Container Name** and clicked **OK** to view the log entries.

Depending on when you check the node will be busy downloading the blockchain or appending MicroBlocks :\)

That’s it. You’ve successfully mastered setting up a LTO Network public node on Alibaba Cloud. Awesome!

## **Step 2: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

