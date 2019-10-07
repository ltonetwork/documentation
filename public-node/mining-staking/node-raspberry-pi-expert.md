---
description: >-
  This page shows the steps needed to get an LTO Network node up and running
  using a Raspberry Pi. Please note that this is a non-straightforward,
  expert-level guide.
---

# Node: Raspberry Pi \(Expert\)

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilise a 2-wallet setup for extra safety.

{% hint style="info" %}
Please note that the recommended manner of running an LTO node is using the provided Docker container. This Raspberry Pi setup guide does not use the container-setup but uses an extract of the core JAR component. Please be advised this is expert-level only.
{% endhint %}

## **Step 0:** Intro to setting up a **Raspberry Pi node**

Although the LTO Public node is available as a JAR \(Java ARchive\), which is usually considered a platform-independent deployment artefact, it is not automatically possible to start this version of the node on a Raspberry PI, e.g., running on Raspbian. This tutorial explains how to modify the corresponding JAR-file in order to allow for a smooth start on a Raspberry Pi.

### **Problem Description**

If you try to start the node via the following command:

```text
$ java -jar lto-public-all.jar lto-mainnet.conf
```

You’ll get an error message similar to the following one:

```text
pi@raspberrypi:~/lto $ java -jar lto-public-all.jar lto-mainnet.conf
2019-04-22 16:09:41,453 INFO  [main] c.w.Application$ - Starting...
2019-04-22 16:09:45,939 INFO  [main] kamon.Kamon$Instance - Initializing Kamon...
2019-04-22 16:09:46,959 INFO  [main] kamon.Kamon$Instance - Kamon-autoweave has been successfully loaded.
2019-04-22 16:09:46,963 INFO  [main] kamon.Kamon$Instance - The AspectJ load time weaving agent is now attached to the JVM (you don't need to use -javaagent).
2019-04-22 16:09:46,968 INFO  [main] kamon.Kamon$Instance - This offers extra flexibility but obviously any classes loaded before attachment will not be woven.
2019-04-22 16:10:08,427 INFO  [ctor.default-dispatcher-3] a.event.slf4j.Slf4jLogger - Slf4jLogger started
2019-04-22 16:10:10,500 INFO  [ctor.default-dispatcher-4] a.event.slf4j.Slf4jLogger - Slf4jLogger started
2019-04-22 16:10:10,535 INFO  [main] c.w.Application$ - LTO v1.0.3 Blockchain Id: L
2019-04-22 16:10:11,158 ERROR [main] c.w.actor.RootActorSystem$ - Error while initializing actor system wavesplatform
java.lang.Exception: Could not load any of the factory classes: org.fusesource.leveldbjni.JniDBFactory, org.iq80.leveldb.impl.Iq80DBFactory
 at com.wavesplatform.db.LevelDBFactory$.$anonfun$load$5(LevelDBFactory.scala:35)
 at scala.Option.getOrElse(Option.scala:121)
 at com.wavesplatform.db.LevelDBFactory$.load(LevelDBFactory.scala:35)
 at com.wavesplatform.db.LevelDBFactory$.factory$lzycompute(LevelDBFactory.scala:10)
 at com.wavesplatform.db.LevelDBFactory$.factory(LevelDBFactory.scala:10)
 at com.wavesplatform.db.package$.openDB(package.scala:22)
 at com.wavesplatform.Application.<init>(Application.scala:54)
 at com.wavesplatform.Application$.$anonfun$main$3(Application.scala:413)
 at com.wavesplatform.Application$.$anonfun$main$3$adapted(Application.scala:387)
 at com.wavesplatform.actor.RootActorSystem$.start(RootActorSystem.scala:25)
 at com.wavesplatform.Application$.main(Application.scala:387)
 at com.wavesplatform.Application.main(Application.scala)
pi@raspberrypi:~/lto $
```

The reason for the error is that one of the dependencies of the node, namely the LevelDB implementation, is not compiled for the ARM architecture of the Raspberry Pi. Therefore, what we need to do in order to start it successfully is described in the next section.

## **Step 1: Getting our node up-and-running**

The basic idea of the patch is to exchange the platform dependent, and not compatible, parts of the JAR-file with corresponding parts that are compatible with the ARM architecture of the Raspberry Pi. In order to perform the next steps in a clean environment, you should copy the **lto-public-all.jar** file to a new directory from which you execute the described commands.

Throughout this setup-guide we utilize the following Github repository: [https://github.com/legalthings/docker-public-node](https://github.com/legalthings/docker-public-node)

First of all, we need to install the platform independent version of the LevelDB database with the following command:

```text
$ sudo apt install libleveldb-java libleveldb-api-java
```

The second step is to unpack the JAR archive. Basically, JAR archives are simple ZIP files with a special directory structure. Therefore, we can just unpack the JAR archive with the following command:

```text
$ jar -xvf lto-public-all.jar
```

After unpacking, we can remove the JAR archive itself, so that we later on, when we repackage the archive, do not include the old archive in the new one:

```text
$ rm lto-public-all.jar
```

Then we secure the MANIFEST.MF file, which is sort of the configuration file of the archive, so that we can later on restore it in order to keep all information about the archive, e.g., which kind of class should be executed once we start the archive. Therefore, we copy the META-INF/MANIFEST.MF file to the current directory:

```text
$ cp META-INF/MANIFEST.MF .
```

The next step is to remove all traces of the platform dependent version of the LevelDB packages:

```text
$ rm -rf `find . -name *leveldb*`
```

Now we just need to extract the platform independent version of the LevelDB installation that we did in the first step. In order to do so, we need to copy two files in our current directory:

```text
$ cp /usr/share/java/leveldb-api.jar .
$ cp /usr/share/java/leveldb.jar .
```

And again, we need to unpack those archives in the current directory:

```text
$ jar -xvf leveldb-api.jar
$ jar -xvf leveldb.jar
```

Now, we can again remove those two archive files in order not to include those as archives in the new node archive that we will create in the last step:

```text
$ rm -f *.jar
```

Finally, we need to recover the MANIFEST.MF file that we have secured before:

```text
$ cp MANIFEST.MF META-INF/
```

before we can finally package the content of our current directory into a new archive that will then be our JAR-archive that we can start the node with:

```text
$ jar -cfm lto-public-all-arm.jar META-INF/MANIFEST.MF *
```

This process might take some time. After it finishes, the created JAR-archive lto-public-all-arm.jar can be copied to whatever directory you want to start your node from. The following command will then start your node:

```text
$ java -jar lto-public-all-arm.jar lto-mainnet.conf
```

### Final considerations

Since a Raspberry Pi has very limited ressources, both from a computational as well as from a memory \(RAM\) point of view, one can not assume that a node on a Raspberry Pi will run with high performance. Nevertheless, it might make sense in some scenarios, e.g., providing an API to the LTO network, running a test environment or providing a testnet node.

The steps described above should not only work on a Raspberry Pi but should also create deployment artifact for other ARM based boards. Depending on the underlying operating systems, some tweaks may be necessary though.

That’s it. You’ve successfully mastered setting up an LTO Network public node on your Raspberry Pi. Awesome!

## **Step 2: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

