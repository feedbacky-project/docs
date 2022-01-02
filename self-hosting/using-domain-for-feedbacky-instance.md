# Using a domain

After having your newly self-hosted Feedbacky instance up and running, you might be wondering; "How can I point a domain to my board instead of having to use my own IP address?" 

In this guide, we will show you how you can use reverse proxy to change that.

{% hint style="warning" %} This guide was written assuming you are using Ubuntu 20.04 as your main OS or any other Debian-based distribution. {% endhint %}

## Apache

### Installing
Before we proceed installing the Apache package, make sure you that everything is up to date:

```text
sudo apt update -y && sudo apt upgrade -y
```

Next, we'll install Apache:

```text
sudo apt install apache2 -y
```

### Creating a new VirtualHost

VirtualHosts allows you to run multiple website on a single webserver (website1.com, website2.com, etc..) this is useful especially if you are also hosting other web services such as a website or store front.

Access Apache's VirtualHost directory:

```text
cd /etc/apache2/sites-available
```

Create a new file with the name being the domain that you will use, be aware that the file name must end with `.conf`.

```text
sudo touch {DOMAIN_NAME}.conf
```

For example, let's say that our fully qualified domain name (FQDN) is `cool.app` and that we want to use the subdomain `feedbacky`, the correct format will look like this:
```text
sudo touch feedbacky.cool.conf
```

Edit your newly made VirtualHost file and paste the configuration below:

{% hint style="info" %} TIP: When creating a file you can directly skip having to use `touch` by instead using `nano`, it will create the new file for you and let you edit it at the same time. {% endhint %}

```text
<VirtualHost *:80>
      ServerName {DOMAIN_NAME}
      ProxyPreserveHost On
      ProxyRequests Off
      ProxyVia Off
      ProxyPass / {SERVER_IP}:{CLIENT_APP_PORT}/
      ProxyPassReverse / {SERVER_IP}:{CLIENT_APP_PORT}/
</VirtualHost>
```

- **{DOMAIN_NAME}**: Your fully qualified domain name, for example `feedbacky.app.cool`.
- **{SERVER_IP}**: The public IP address used by your hosting machine.
- **{CLIENT_APP_PORT}**: The client port located in your `.env` file, by default it is 8090.

Save the file and exit by doing `CTRL` + `C`.

### Enabling Reverse Proxy

By default Proxy is disabled in Apache, we can enable it by using the following command:

```text
sudo a2enmod proxy
```

To understand what this command does, we can break it down like this,

- a2 is apache2
- en is enable
- mod is module 

We are asking Apache to enable the proxy module which is situated in the `/etc/apache2/available-modules` directory.

### Enabling the VirtualHost

In order to create our Apache VirtualHost, type the following:

```text
sudo a2ensite {DOMAIN_NAME}.conf
```

Finally, using the DNS provider of your choice, create an **A Record** that points to your machine's public IP adress and Apache will handle the for rest for you.

Voila 🎉, you just created a VirtualHost with Apache for your Feedbacky instance!

## NGINX

This guide will be fairly similar to the Apache guide with some obvious variations.

### Installing

Make sure everything is up to date:

```text
sudo apt update -y && sudo apt upgrade -y
```
Next, we'll install NGINX,

```text
sudo apt install nginx -y
```


