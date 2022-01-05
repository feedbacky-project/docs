---
description: Secure your board.
---

# Using TLS/SSL

This guide will show you how you can generate your own TLS/SSL certificate with Certbot.

## Cloudflare

An alternative method that you can follow instead of manually generating a certificate and adding additional values to your VHost is by using Cloudflare.

In short by adding your website to Cloudflare you will be able to forward your board with Cloudflare's own proxy.

You can read more [here](https://support.cloudflare.com/hc/en-us/articles/201720164).

## Installing

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

## Port Forwarding

With UFW, use the following command:

* Forwarding Port 443:

```
sudo ufw allow 443/tcp
```

## Creating a Certificate

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

### Renew your Certificate

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

## Updating your VHost

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

### Additional steps

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
