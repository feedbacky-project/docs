# Domain Setup

{% hint style="info" %}
<mark style="color:blue;">**Work in progress!**</mark>

This section is currently being worked on!
{% endhint %}

This guide will show you how to use the Apache or NGINX web server in order to point a domain to your Feedbacky instance by using reverse proxy.

#### DNS Provider

For your domain to actually work use the DNS provider (Cloudflare, Namecheap, etc..) of your choice and create an **A Record** that points to your machine's public IP address.

You can find your machine's public address by typing:

```
curl ipinfo.io/ip
```

If you use a VPS you will find your public IP address and more information in the dashboard of your provider.

#### Port Forwarding

With UFW, use the following commands:

* Forwarding Port 80:

```
sudo ufw allow 80/tcp
```

#### Creating a new VHost

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

#### Additional steps

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

#### Enabling your VHost

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

#### TLS/SSL

We only covered the basics but if you want to go one step further you can generate your own TLS/SSL certificate for your domain by following the guide below:

This guide will show you how you can generate your own SSL certificate with Certbot.

#### Cloudflare

An alternative method that you can follow instead of manually generating a certificate and adding additional values to your VHost is by using Cloudflare.

In short by adding your website to Cloudflare you will be able to forward your board with Cloudflare's own proxy.

You can read more [here](https://support.cloudflare.com/hc/en-us/articles/201720164).

#### Installing

Before we proceed, make sure you that everything is up to date:

```
sudo apt update -y && sudo apt upgrade -y
```

Installing Certbot:

```
sudo apt install -y certbot
```

Run the follow command for the web server you use:

{% tabs %}
{% tab title="Apache" %}
```
sudo apt install -y python3-certbot-apache
```
{% endtab %}

{% tab title="NGINX" %}
```
sudo apt install -y python3-certbot-nginx
```
{% endtab %}
{% endtabs %}

#### Port Forwarding

With UFW, use the following command:

* Forwarding Port 443:

```
sudo ufw allow 443/tcp
```

#### Creating a Certificate

In order to create a certificate you must use the domain that you previously created for your board in our domain guide.

{% tabs %}
{% tab title="Apache" %}
```
sudo certbot certonly --apache -d {DOMAIN_NAME}
```

{% hint style="success" %}
**Example**

This is how the correct format to use would look like:

```
sudo certbot certonly --apache -d feedbacky.cool.app
```
{% endhint %}
{% endtab %}

{% tab title="NGINX" %}
```
sudo certbot certonly --nginx -d {DOMAIN_NAME}
```

{% hint style="success" %}
**Example**

This is how the correct format to use would look like:

```
sudo certbot certonly --nginx -d feedbacky.cool.app
```
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Standalone**

If neither the Apache nor the NGINX command work, try using the standalone command:

```
sudo certbot certonly --standalone -d {DOMAIN_NAME}
```
{% endhint %}

#### Renew your Certificate

Certbot doesn't automatically renew your certificate, for that you would need [acme.sh](https://github.com/acmesh-official/acme.sh), in order to manually renew your certificate type:

```
certbot renew
```

{% hint style="warning" %}
**Using NGINX?**

When renewing your certificate make sure to stop the services before proceeding, else you will get an error.

* Stop NGINX:

```
sudo systemctl stop nginx
```

* Renew:

```
sudo certbot renew
```

* Start NGINX:

```
sudo systemctl start nginx
```
{% endhint %}

#### Updating your VHost

Once you have generate your certificate you must update your VHost to take TLS/SSL in consideration.

{% tabs %}
{% tab title="Apache" %}
{% hint style="danger" %}
Do not change the `{SERVER_NAME}` value!
{% endhint %}

```
<VirtualHost *:80>
  ServerName {DOMAIN_NAME}
  
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L] 
</VirtualHost>

<VirtualHost *:443>
  ServerName {DOMAIN_NAME}
  
  SSLEngine on
  SSLCertificateFile /etc/letsencrypt/live/{DOMAIN_NAME}/fullchain.pem
  SSLCertificateKeyFile /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem
</VirtualHost> 
```
{% endtab %}

{% tab title="NGINX" %}
```
server {
    listen 80;
    server_name {DOMAIN_NAME};

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name {DOMAIN_NAME};

    ssl_certificate /etc/letsencrypt/live/{DOMAIN_NAME}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1>
    ssl_prefer_server_ciphers on;

    http2_idle_timeout 5m; # up from 3m default
    client_max_body_size 0;

     location / {
       proxy_pass {SERVER_IP}:{CLIENT_APP_PORT};
  }
}
```
{% endtab %}
{% endtabs %}

#### Additional steps

{% tabs %}
{% tab title="Apache" %}
* By default SSL is disabled in Apache, we can enable it by using the following command:

```
sudo a2enmod ssl
```

* Restart your Apache service:

```
sudo systemctl restart apache2
```
{% endtab %}
{% endtabs %}
