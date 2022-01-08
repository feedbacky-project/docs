---
description: Using a cool domain with your Feedbacky instance.
---

# Using a Domain

This guide will show you how to use the Apache or NGINX web server in order to point a domain to your Feedbacky instance by using reverse proxy.

## DNS Provider

For your domain to actually work use the DNS provider (Cloudflare, Namecheap, etc..) of your choice and create an **A Record** that points to your machine's public IP address.

You can find your machine's public address by typing:

```
curl ipinfo.io/ip
```

If you use a VPS you will find your public IP address and more information in the dashboard of your provider.

## Port Forwarding

With UFW, use the following commands:

* Forwarding Port 80:

```
sudo ufw allow 80/tcp
```

## Creating a new VHost

A "VirtualHost" or "VHost" abridged, makes it possible to run multiple website on a single web server (website1.com, website2.com, etc..) this is useful especially if you are also hosting other web services such as a website or store front.

{% tabs %}
{% tab title="Apache" %}
1\. Access Apache's VHost directory:

```
cd /etc/apache2/sites-available
```



2\. Create a new file with the name being the domain that you will use, be aware that the file name must end with ".conf".

{% hint style="info" %}
**TIP**

When creating a file, you can directly skip having to use **touch** by instead using **nano**, it will create a new file for you and let you edit it at the same time.
{% endhint %}

```
sudo touch {DOMAIN_NAME}.conf 
```

{% hint style="success" %}
**Example**

Let's say that our domain name is **cool.app** and that we want to use the subdomain **feedbacky**, the correct format to use would look like this:

```
sudo touch feedbacky.cool.conf 
```
{% endhint %}



3\. Edit your newly made Apache VirtualHost file and paste the configuration below:

{% code title="{DOMAIN_NAME}.conf" %}
```apacheconf
<VirtualHost *:80>
  ServerName {DOMAIN_NAME}
  ProxyPreserveHost On
  ProxyRequests Off
  ProxyVia Off
  ProxyPass / {SERVER_IP}:{CLIENT_APP_PORT}/
  ProxyPassReverse / {SERVER_IP}:{CLIENT_APP_PORT}/
</VirtualHost> 
```
{% endcode %}



| `{DOMAIN_NAME}`     | Your fully qualified domain domain, for example feedbacky.app.cool. |
| ------------------- | ------------------------------------------------------------------- |
| `{SERVER_UP}`       | The public IP address used by your hosting machine.                 |
| `{CLIENT_APP_PORT}` | The client port located in your `.env` file, by default it is 8090. |



4\. You can now save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.
{% endtab %}

{% tab title="NGINX" %}
1\. Access NGINX's VHost directory:

```
cd /etc/nginx/sites-available
```



{% hint style="info" %}
**TIP**

When creating a file, you can directly skip having to use **touch** by instead using **nano**, it will create a new file for you and let you edit it at the same time.
{% endhint %}



2\. Create a new file with the name being the domain that you will use, be aware that the file name must end with ".conf".

```
sudo touch {DOMAIN_NAME}.conf 
```

{% hint style="success" %}
**Example**

Let's say that our domain name is **cool.app** and that we want to use the subdomain **feedbacky**, the correct format to use would look like this:

```
sudo touch feedbacky.cool.conf 
```
{% endhint %}



3\. Edit your newly made VirtualHost file and paste the configuration below:

```
server {
    listen 80;
    server_name {DOMAIN_NAME};

     location / {
       proxy_pass {SERVER_IP}:{CLIENT_APP_PORT};
  }
}
```

| `{DOMAIN_NAME}`     | Your fully qualified domain domain, for example feedbacky.app.cool. |
| ------------------- | ------------------------------------------------------------------- |
| `{SERVER_IP}`       | The public IP address used by your hosting machine.                 |
| `{CLIENT_APP_PORT}` | The client port located in your `.env` file, by default it is 8090. |



4\. You can now save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.
{% endtab %}
{% endtabs %}

### Additional steps

This sections contains some additional steps you need to follow before enabling your VHost.

{% tabs %}
{% tab title="Apache" %}
5\. By default Proxy is disabled in Apache, we can enable it by using the following command:

```
sudo a2enmod proxy
```

{% hint style="success" %}
**Breakdown**

To understand what this command does, we can break it down like this,

* **a2** means apache2
* **en** means enable
* **mod** means module

We are asking Apache to enable the proxy module which is located in the`/etc/apache2/available-modules` directory.
{% endhint %}
{% endtab %}

{% tab title="NGINX" %}
5\. NGINX will only enable your VHost if you create a symbolic link (symlink in short), it is a shortcut to another file

```
sudo ln -s /etc/nginx/sites-available/{DOMAIN_NAME}.conf /etc/nginx/sites-enabled/{DOMAIN_NAME}.conf
```

{% hint style="success" %}
**Breakdown**

Each time you will edit your VHost file in `/etc/nginx/sites-available` it will also update the one created in `/etc/nginx/sites-enabled`.&#x20;
{% endhint %}
{% endtab %}
{% endtabs %}

### Enabling your VHost

Title self-explanatory, these steps will enable your newly made VHost file.

{% tabs %}
{% tab title="Apache" %}
6\. For Apache to enable your VHost type this:

```
sudo a2ensite {DOMAIN_NAME}.conf
```
{% endtab %}

{% tab title=" NGINX" %}
6\. For NGINX to enable your VHost type this:

```
sudo service nginx reload
```
{% endtab %}
{% endtabs %}

Congratulation! 🎉, you've just created a VHost for your Feedbacky instance!

## TLS/SSL

We only covered the basics but if you want to go one step further you can generate your own TLS/SSL certificate for your domain by following the guide below:

{% content-ref url="using-tls-ssl.md" %}
[using-tls-ssl.md](using-tls-ssl.md)
{% endcontent-ref %}

&#x20;
