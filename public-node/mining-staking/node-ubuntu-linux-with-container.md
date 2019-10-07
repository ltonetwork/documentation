---
description: >-
  This page explains all the steps required to get your node up and running
  using a Ubuntu Linux (virtual) server.
---

# Node: Ubuntu Linux with Container

So you decided you want to be part of the LTO Network, awesome!

A great way to be part of the community is by actively participating as a node in the network. 

Make sure to check out the Prepare: [Setup your wallet page](https://app.gitbook.com/@ltonetwork/s/project/~/edit/drafts/-LfnlY2o1T-oAq3ytEpO/community-area/mining-staking/prepare-setup-your-wallet) before continuing. The node setup pages assume you utilize a 2-wallet setup for extra safety.

## Step 0: Intro to setting up a secure Ubuntu server for the first time

This tutorial is meant for beginners who want to set up an Ubuntu server for the first time. It contains the very basic steps of installing Ubuntu on a new server, enhance security and installing the LTO Network Public node. The tutorial is based on Ubuntu 16.04. It is recommended to run nodes on a rented VPS. Most providers offer a single click to install an OS. Things may differ depending on your provider.

{% hint style="info" %}
Replace everything between **&lt; &gt;** with your own values.
{% endhint %}

To install Ubuntu, use the following official tutorial which will guide you through the process. [https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-server-1604](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-server-1604)

![Make sure to install OpenSSH in step 10 of the official installation proces. Others are optional.](https://cdn-images-1.medium.com/max/1600/1*hhyjlitkKdf4Eem0NxV__Q.png)

We’re using [SSH](https://en.wikipedia.org/wiki/Secure_Shell) to connect to our server. For Windows use either PowerShell \(run as administrator\) or [PuTTy](https://www.putty.org/).

Mac users can use the built-in terminal \(spotlight, command + spacebar: search for terminal\)

{% hint style="info" %}
It is most likely that you’ve created a user with your own username during the installation and you’ll never have to login as ‘root’. If this is the case for you, you can skip to step 3.
{% endhint %}

By default, the root account password is locked in Ubuntu. This means that you cannot login as root directly or use the su command to become the root user. However, since the root account physically exists it is still possible to run programs with root-level privileges. This is where sudo comes in — it allows authorized users to run certain programs as root without having to know the root password. **If you are, for some reason, logging in as root, please first follow step 1 and 2.**

## Step 1: Basic system setup as root

If you are done with the installation, connect to your server through SSH, using the root account provided by your host.

```text
$ ssh root@<ip-address>
example: ssh root@13.37.13.37
```

Change the root password \(type your new password twice\):

```text
# passwd
```

Login again to your system using a different terminal to make sure your password change was successful.

```text
$ ssh @<ip-address>
example: ssh root@13.37.13.37
```

## Step 2: Adding a new user

Now, we’re going to create a new regular user as it is not recommended to use the ‘root’ account for your node.

```text
# adduser <new_user>
example: adduser john
```

Enter a new and strong password twice and press enter six times to accept the default values.

Now we’re going to add the new user to the ‘sudo’ group. sudo allows a permitted user to execute a command as the superuser or another user, as specified by the security policy.

```text
# usermod -a -G sudo <new_user>
example: sudo usermod -a -G sudo john
```

Exit and connect to your server as the new user, not as root. Use the password you’ve set while creating the new user.

```text
# exit
$ ssh <new_user>@<ip-address>
example: ssh john@13.37.13.37
```

### **Step 2.1: Extra security measure - add SSH key to your user**

{% hint style="info" %}
While it is possible to manage your servers using password-based logins, it is often a better idea to set up and use SSH key pairs. SSH keys are more secure than passwords, and can help you login without having to remember long passwords.
{% endhint %}

More info and extensive guide: [https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)

## Step 3: Adding our user to the sudo group

If you’ve followed step 1 and 2, you can now skip to step 4. If you are done with the installation, connect to your server through SSH, using the account you have set up during the installation process.

```text
$ ssh john@<ip-address>
example: ssh john@13.37.13.37
```

We need to add the user to the ‘sudo’ group. sudo allows a permitted user to execute a command as the superuser or another user, as specified by the security policy.

```text
$ sudo usermod -a -G sudo <user>
example: sudo usermod -a -G sudo john
```

## Step 4: Updating and installing packages on your server

The following commands require root privileges. To grant root privileges, simply prepend sudo to all the commands you need to run as root. If you ever get a ‘permission denied’ error, you probably forgot to prepend sudo to the command. Example: ‘sudo apt-get install npm’. Whenever you get the question if you want to continue and additional disk space will be used, just press Y on your keyboard.

It is important to update all the existing packages on the server. Ubuntu will ask you to fill in your password once again since you are now logged in as a ‘regular’ user.

To update the packages, use the following commands as the root user \(and repeat those commands at least every week to get the latest security updates\):

```text
$ sudo apt-get update
$ sudo apt-get dist-upgrade
```

Reboot your server \(just in case the kernel has been updated\) and connect again.

```text
$ sudo reboot
$ ssh <user>@<ip-address>
example: ssh john@13.37.13.37
```

It is important to have an accurate time on your system as this sometimes can cause for conflicts. In most cases it’s best to use pool.ntp.org to find an NTP server. The system will try finding the closest available servers for you.

```text
$ sudo apt-get install ntp
$ sudo apt-get install ntpdate
$ sudo service ntp stop
$ sudo ntpdate pool.ntp.org
$ sudo service ntp start
```

Now, install additional packages which are needed or useful. We install nano text editor to edit the docker-compose file later in this tutorial.

```text
$ sudo apt-get install nano
```

## Step 5: Secure your SSH connection

We’re going to change the standard SSH port to make your server just a bit more secure.

{% hint style="info" %}
To make it even more secure, you can use the following guide \(not recommended\): https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/

We’re not recommending this, as you won’t be able to login anymore from any computer or mobile phone on the go.
{% endhint %}

Open the sshd\_config file with nano:

```text
$ sudo nano /etc/ssh/sshd_config
```

Go to the line with ‘Port 22’ and change it to another port number between 49152 and 65535.

```text
example ‘Port 51234’
```

From now on, we will be referring to this new port as &lt;new\_ssh\_port&gt;.

Go to line ‘PermitRootLogin yes’ and change it to ‘PermitRootLogin no’.

```text
example ‘PermitRootLogin no’
```

Press control + X, press Y and press enter to save your new configuration.

Restart the SSH service:

```text
$ sudo service ssh restart
```

Now we’re going to disconnect and reconnect again to test if the new port is working and remember to add your new port as following:

```text
$ exit
$ ssh -p <new_ssh_port> <user>@<ip-address>
example: ssh -p 51234 john@13.37.13.37
```

## Step 6: Enabling Swap space

{% hint style="info" %}
It’s not recommended to enable swap with SSD drives as it can cause drive degradation over time.
{% endhint %}

It is recommended to enable Swap on your server if you don’t have an SSD drive, if not done already.

First check if swap isn’t already enabled:

```text
$ sudo swapon -s
$ free -m
```

If swap is not enabled, use the following extensive guide to enable it: [https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)

## Step 7: Installing fail2ban

Fail2ban scans log files and bans IPs that show the malicious signs: too many password failures, seeking for exploits, etc. Generally Fail2Ban is then used to update firewall rules to reject the IP addresses for a specified amount of time.

```text
$ sudo apt-get install fail2ban
```

Now we’re going to edit the fail2ban config to your modified ssh port:

```text
$ sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
$ sudo nano /etc/fail2ban/jail.local
```

Replace every line that looks like this:

```text
port = ssh
```

With your new ssh port:

```text
port = <new_ssh_port>
```

It should look then similar to this:

```text
example:
#
# SSH servers
#
[sshd]
port = 51234
logpath = %(sshd_log)s
```

Save and exit with ctrl + X, Y, ENTER.

Now, restart fail2ban:

```text
$ sudo service fail2ban restart
```

## Step 8: Setting up your firewall

Check this tutorial fore more details: [https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04)

First check the status of the firewall:

```text
$ sudo ufw status
```

Now we’re going to set up the firewall, blocking all the ports but allowing traffic on the ports for your SSH connection and opening the port for synchronization with other nodes in the network.

```text
$ sudo ufw disable
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
$ sudo ufw allow 6868
$ sudo ufw allow <sshport>
example: sudo ufw allow 51234
```

```text
$ sudo ufw logging on
$ sudo ufw enable
```

Check the status again to see if the rules are updated:

```text
$ sudo ufw status
```

After a short period of time you can reconnect to your VPS.

## Step 9: Installing Docker

We’re finally getting close to installing your LTO Network Public node! The node is easily installed through a Docker image, so first we’re going to need to install Docker & Docker Compose on our server. Use the following guides to do so and stop when you’ve ran the test provided in this guide \(sudo docker run hello-world\):

* [ ] Docker: [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [ ] Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Now, give docker the proper privileges:

```text
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```

Log out and log back in so that your group membership is re-evaluated.

```text
$ exit
$ ssh -p <new_ssh_port>@<ip-address>
example: ssh -p 51234 john@13.37.13.37
```

Verify that you can run docker commands without sudo.

```text
$ docker run hello-world
```

For more info check: [https://docs.docker.com/install/linux/linux-postinstall/\#manage-docker-as-a-non-root-user](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)

## Step 10: Starting your node for the first time

Yay, it’s finally time to start your node! At least you have a more secure server now, and that is super important!

Head over to the LTO.network Github repository: [https://github.com/ltonetwork/lto-public-node/blob/master/docker-compose.yml](https://github.com/ltonetwork/lto-public-node/blob/master/docker-compose.yml) and open the default Docker Compose file.

Look up your IP address by entering the following command in your command line, copy the IP address and save it somewhere, you will need it in the next step.

```text
$ curl https://api.ipify.org
```

Now, all you need is the docker-compose.yml file. What works best is copying the content in a new file. So let’s make the new file on your server:

```text
$ sudo touch docker-compose.yml
```

Open the file with nano:

```text
$ sudo nano docker-compose.yml
```

Paste the contents from the docker-compose.yml file from the LTO Github repository in a code editor like Visual Studio Code and enter your details. Now copy it and paste the contents in the file in your node with your right mouse click. Example:

```text
version: '3'
```

```text
services:
```

```text
public-node:
    container_name: public-node
    image: legalthings/public-node
    ports:
      - 6869:6869
      - 6868:6868
```

```text
    environment:
      - LTO_WALLET_SEED=<place the fifteen words here>
      - LTO_WALLET_PASSWORD=<setapasswordforencryption>
      - LTO_NETWORK=MAINNET
      - LTO_DECLARED_ADDRESS=<your_node_ip>:6868
```

_**\(not mandatory\)**_ If you want to approach your node from your localhost with a browser, add the following lines after the last line:

```text
- LTO_ENABLE_REST_API=true
- LTO_API_KEY=<setapasswordforyourapi>
```

Press ctrl +x and y to close and save the file.

**Now it’s really time to start your node! Use the following command to run the Docker container in the background. It will pull the latest image from the LTO node and start running.**

```text
$ docker-compose up -d
```

To see the progress use the following command:

```text
$ docker logs -f public-node
```

To leave the node running in the background either close the terminal or press ctrl + c.

{% hint style="info" %}
If you get an ‘unsupported version’ error after starting your node, change the version number to ‘2’ in the docker-compose.yml file.
{% endhint %}

That’s it. You’ve successfully mastered setting up a LTO Network public node on your own Linux server. Awesome!

## **Step 11: Wait for a 1.000 blocks**

You can find the LTO Network Explorer at [https://explorer.lto.network.](https://explorer.lto.network./) It shows you the blocks generated, by who, when, how big they are and how many transactions are in the block.

![](../../.gitbook/assets/image%20%282%29.png)

After launching your node check the [Explorer](https://explorer.ltonetwork.com/dashboard) to see the number of the last block. Wait till another 1.000 blocks are generated and expect your node to be part of the LTO Network.

{% hint style="info" %}
Utilize the available [Community Tech Tools](../network-overview-tools.md) to get more insights into the network and your participation.
{% endhint %}

Depending on your stake \(the number of LTO tokens you have in the “2nd wallet”\) it will take more or less time for you to start earning LTO.

Be patient and be happy. Welcome to the amazing LTO Network community.

