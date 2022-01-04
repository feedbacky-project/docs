# Reverse Proxy with Apache

The Apache HTTP Server is a free and open-source cross-platform web server software, released under the terms of Apache License 2.0. Apache is developed and maintained by an open community of developers under the auspices of the Apache Software Foundation.

(_Source_ [_Wikipedia_](https://en.wikipedia.org/wiki/Apache\_HTTP\_Server))

## Installing

Before we proceed installing the Apache package, make sure you that everything is up to date:

```
sudo apt update -y && sudo apt upgrade -y
```

Next, we'll install Apache:

```
sudo apt install apache2 -y
```

#### Creating a new VirtualHost

VirtualHosts allows you to run multiple website on a single web server (website1.com, website2.com, etc..) this is useful especially if you are also hosting other web services such as a website or store front.

Access Apache's VirtualHost directory:

```
cd /etc/apache2/sites-available
```

Create a new file with the name being the domain that you will use, be aware that the file name must end with ".conf".

```
sudo touch {DOMAIN_NAME}.conf 
```

For example, let's say that our fully qualified domain name (FQDN) is **cool.app** and that we want to use the subdomain **feedbacky**, the correct format will look like this:

```
sudo touch feedbacky.cool.conf 
```

Edit your newly made VirtualHost file and paste the configuration below:

{% hint style="info" %}
TIP: When creating a file you can directly skip having to use `touch` by instead using `nano`, it will create the new file for you and let you edit it at the same time.
{% endhint %}

```
<VirtualHost *:80>
      ServerName {DOMAIN_NAME}
      ProxyPreserveHost On
      ProxyRequests Off
      ProxyVia Off
      ProxyPass / {SERVER_IP}:{CLIENT_APP_PORT}/
      ProxyPassReverse / {SERVER_IP}:{CLIENT_APP_PORT}/
</VirtualHost>
```

| Value               | Description                                                         |      |
| ------------------- | ------------------------------------------------------------------- | ---- |
| `{DOMAIN_NAME}`     | Your fully qualified domain domain, for example feedbacky.app.cool. | You  |
| `{SERVER_IP}`       | The public IP address used by your hosting machine.                 |      |
| `{CLIENT_APP_PORT}` | The client port located in your `.env` file, by default it is 8090. |      |

You can now save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.

#### Enabling Reverse Proxy

By default Proxy is disabled in Apache, we can enable it by using the following command:

```
sudo a2enmod proxy
```

To understand what this command does, we can break it down like this,

{% tabs %}
{% tab title="a2" %}
Is apache2.
{% endtab %}

{% tab title="en" %}
Is enable.
{% endtab %}

{% tab title="mod" %}
Is module.
{% endtab %}
{% endtabs %}

We are asking Apache to enable the proxy module which is located in the `/etc/apache2/available-modules` directory.

#### Enabling the VirtualHost

In order to create our Apache VirtualHost, type the following:

```
sudo a2ensite {DOMAIN_NAME}.conf
```

Finally, using the DNS provider of your choice, create an **A Record** that points to your machine's public IP adress and Apache will handle the for rest for you.

Voila 🎉, you just created a VirtualHost with Apache for your Feedbacky instance!
