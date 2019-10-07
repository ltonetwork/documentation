---
description: >-
  This page shows the steps required to setup an Nginx reverse proxy to securely
  access your node's API remotely.
---

# Optional: Nginx reverse proxy

LTO Network nodes have a cool web interface where you can check info about your node, network and even sign and send transactions. It also serves as a REST API with its own Swagger documentation.

Once you have your LTO Node configured and running, you should be able to access the web interface with the following url: [http://localhost:6869](http://localhost:6869/)

![](https://www.dumbitcoin.com/wp-content/uploads/2019/01/main-1024x612.png)

Without a graphical interface, e.g in a VPS, doing a curl http://localhost:6869 serves to check if your node has the web interface enabled. If it is not, the response is a connection refused error.

If your web interface is not working, the reason is that the following lines are missing from your Docker config file. Add them to enable the API:

```text
- LTO_ENABLE_REST_API=true
- LTO_API_KEY=<somestrongpassword>
```

The second line is optional and intended to be used solely for executing privileged actions from the web interface.

Do not forget to rebuild the image when the config file is changed

```text
$ docker-compose down 
$ docker-compose up
```

At this point you might be wondering: How do I access this web interface from outside the network?

That is the exact purpose of this tutorial. The following paragraph shows how to configure Nginx as a reverse proxy to access the LTO node web interface from the internet securely without opening any port. 

## Step 1: Setup our Nginx reverse proxy

First of all Nginx must be installed

```text
$ sudo apt update
$ sudo apt install nginx
```

If everything is installed correctly, you should see that Nginx service is active with this command

```text
$ systemctl status nginx
```

![](https://www.dumbitcoin.com/wp-content/uploads/2019/01/Sin-t%C3%ADtulo.jpg)

Also, if you paste your public IP in any browser, the Nginx default page should appear.

Now w move to create our reverse proxy.

We need to edit a file located in /etc/nginx/sites-available/default, delete everything and paste the following text:

```text
server {
    listen 80 ;
    location / {
        proxy_pass http://localhost:6869;
    }
} 
```

Finally, restart Nginx to apply changes

```text
$ sudo systemctl restart nginx
```

At this moment, the reverse proxy should be working. Paste the public IP of your node machine in any browser and should be visible and working.

It is strongly recommended to use secure connections using a SSL/TLS certificate when managing API keys. Continue reading to improve the security.

## **Step 2: Adding HTTPS support to the reverse proxy**

In order to do this, we need to have a registered domain name pointing to the public node IP. There are many places where you can get really cheap domains, even free. SSL certificates prohibits to be assigned directly to IP addresses, so having a domain name is a requisite here.

We will use the well known certificate generator Certbot. To install it:

```text
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx 
```

Before executing Certbot, we must set our domain name into Nginx. At the second line in /etc/nginx/sites-available/default, insert:

```text
server_name yourdomainforlto.com;
```

Then restart Nginx to apply changes

```text
$ sudo systemctl restart nginx
```

Launch Certbot and follow the process that is short and straightforward. It will ask your email to be notified for renewals and alerts. Select the option to redirect to HTTPS if you want to use only HTTPS \(recommended\).

```text
$ sudo certbot --nginx -d yourdomainforlto.com
```

In some cases, Certbot will throw a firewall error if your system has ufw firewall enabled. In order to solve this, allow Nginx with the following command

```text
$ sudo ufw allow 'Nginx Full'
$ sudo ufw delete allow 'Nginx HTTP'
```

There is a more complete tutorial about Nginx, Certbot and Ufw [here](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04).

At this point, Certbot has generated a Let’s Encrypt SSL certificate for your site and also configured your Nginx secure reverse proxy. Now you should be able to enter your node web interface from your custom domain name securely through HTTPS, congratulations!

![](https://www.dumbitcoin.com/wp-content/uploads/2019/01/certf.jpg)

## **Optional: Setting a login and password for your node web interface**

This is an extra security measure, setting a login and password for your site will allow you to give access only to the people with this information. It doesn’t matters if you have configured a SSL certificate or not, it will work anyway. In order to do this, enter the following commands. LTOuser is an example username. It will ask you for the password to set.

```text
$ sudo sh -c "echo -n 'LTOuser:' >> /etc/nginx/.htpasswd"
$ sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"
```

 Finally, edit again the file /etc/nginx/sites-available/defaultand add the following lines under the proxy\_pass line for example

```text
auth_basic "LTOuser";
auth_basic_user_file /etc/nginx/.htpasswd;
```

 Don’t forget to restart Nginx to apply changes

```text
$ sudo systemctl restart nginx
```

 At this moment, when someone tries to enter into your node web interface, the browser will prompt a message to enter the authentication data. 

![](https://www.dumbitcoin.com/wp-content/uploads/2019/01/Sin-t%C3%ADtulolto.jpg)

