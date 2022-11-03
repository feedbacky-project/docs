---
description: Use your own domain with Feedbacky.
---

# Webserver Setup

{% hint style="warning" %}
Content being rewritten, some information might be missing.
{% endhint %}

## Record

Before going into details, you must have your server pointed to your domain.

1\. Access the DNS settings of your domain provider.

2\. Create an **A Record**.

3\. Find your public IP address.

```bash
curl ipinfo.io/ip
```

4\. Insert a subdomain, for example; **feedbacky**.

5\. Put your public IP address in the address field.

## Port Forwarding

## Firewall setup

Uncomplicated Firewall (UFW) is the default Firewall on Ubuntu 20.04, it is disabled by default.

1\. Enable Uncomplicated Firewall (UFW).

```bash
sudo ufw enable
```

2\. Forward the Feedbacky server port.

```bash
sudo ufw allow 8095, 80, 443 proto tcp
```

{% hint style="info" %}
If you plan on using a different port, make sure that you change the value of the `SERVER_APP_PORT` variable when [configuring](configuring.md)!
{% endhint %}

{% hint style="success" %}
Make sure that you also create a port forwarding rule in your router settings page. If you are using a Virtual Private Server (VPS) check with your provider.
{% endhint %}

## Virtual Hosts

### Creating

Virtual Hosts enables you to run multiple website on a single webserver, this is useful if you are also hosting other services such as a store front, documentation site, etc..

1\. Access your webserver's Virtual Host directory.

{% tabs %}
{% tab title="Apache" %}
```bash
cd /etc/apache2/sites-available
```
{% endtab %}

{% tab title="Nginx" %}
```bash
cd /etc/nginx/sites-available
```
{% endtab %}
{% endtabs %}

2\. Create a new file with the name being your domain, the file name must end with ".conf".

```
sudo nano {feedbacky.yourdomain}.conf
```

| `{feedbacky.yourdomain}.conf` | Your domain including the subdomain, example; **feedbacky.myapp.conf**. |
| ----------------------------- | ----------------------------------------------------------------------- |

3\. Paste the following content to your newly created Virtual Host file.

{% tabs %}
{% tab title="Apache" %}
{% code title="{feedbacky.yourdomain}.conf" %}
```apacheconf
<VirtualHost *:80>
  ServerName {DOMAIN_NAME}
</VirtualHost>

# Comment out (Adding #) to the part below if you don't plan on using SSL!
<VirtualHost *:443>
  ServerName {DOMAIN_NAME}
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L] 
  SSLEngine on
  SSLCertificateFile /etc/letsencrypt/live/{DOMAIN_NAME}/fullchain.pem
  SSLCertificateKeyFile /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem
</VirtualHost> 
```
{% endcode %}
{% endtab %}

{% tab title="Nginx" %}
{% code title="{feedbacky.yourdomain}.conf" %}
```nginx
server {
    listen 80;
    server_name {DOMAIN_NAME};

    return 301 https://$host$request_uri;
}

# Comment out (Adding #) to the part below if you don't plan on using SSL!
server {
    listen 443 ssl http2;
    server_name {DOMAIN_NAME};

    ssl_certificate /etc/letsencrypt/live/{DOMAIN_NAME}/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES2>-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";
    ssl_prefer_server_ciphers on;

    http2_idle_timeout 5m; # up from 3m default
    client_max_body_size 0;

     location / {
       proxy_pass {SERVER_IP}:{CLIENT_APP_PORT};
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

| `{DOMAIN_NAME}`       | Your fully qualified domain domain, example; `feedbacky.app.cool`.                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `{SERVER_IP}`\*       | <p>*<em><strong>Nginx</strong></em></p><p>The IP address (not public) of your Virtual Private Server (VPS) or dedicated hardware.</p> |
| `{CLIENT_APP_PORT}`\* | <p>*<em><strong>Nginx</strong></em></p><p>The client port located in your <code>.env</code> file, by default it is 8090.</p>          |

4\. Save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.

### Additional steps

{% tabs %}
{% tab title="Apache" %}
5\. By default `proxy` and `ssl` are disabled in Apache, enable them.

```bash
sudo a2enmod proxy ssl
```

***

{% hint style="info" %}
<mark style="color:blue;">**Breakdown**</mark>

To understand what this command does, we can break it down like this,

* **a2** means apache2
* **en** means enable
* **mod** means module

We are asking Apache to enable the proxy module which is located in the`/etc/apache2/available-modules` directory.
{% endhint %}
{% endtab %}

{% tab title="Nginx" %}
5\. A symbolic link (symlink) must be created for making your Virtual Host work.

```bash
sudo ln -s /etc/nginx/sites-available/{DOMAIN_NAME}.conf /etc/nginx/sites-enabled/{DOMAIN_NAME}.conf
```



{% hint style="info" %}
<mark style="color:blue;">**Breakdown**</mark>

Each time you will edit your Virtual Host located in `/etc/nginx/sites-available` it will also update the one created in `/etc/nginx/sites-enabled`.
{% endhint %}
{% endtab %}
{% endtabs %}

### Enabling

{% tabs %}
{% tab title="Apache" %}
```bash
sudo a2ensite {feedbacky.yourdomain}.conf
```
{% endtab %}

{% tab title=" Nginx" %}
```
sudo service nginx reload
```
{% endtab %}
{% endtabs %}

## Generating a Certificate

Generate your domain SSL certificate.

{% tabs %}
{% tab title="Apache" %}
```
sudo certbot certonly --apache -d {DOMAIN_NAME}
```
{% endtab %}

{% tab title="Nginx" %}
```
sudo certbot certonly --nginx -d {DOMAIN_NAME}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
<mark style="color:orange;">**Standalone**</mark>

If neither the Apache nor the Nginx command worked, try using the standalone command.

```
sudo certbot certonly --standalone -d {DOMAIN_NAME}
```
{% endhint %}

If everything went well, you should now have your own self-signed certificate for your domain.

## Let's Encrypt Alternative

### Cloudflare

An alternative method that you can follow instead of manually generating a certificate and adding additional values to your Virtual Host file is by using Cloudflare.

In short by adding your website to Cloudflare, you will be able to forward your board with Cloudflare's own proxy.

You can read more [here](https://support.cloudflare.com/hc/en-us/articles/201720164).

{% hint style="warning" %}
The normal domain setup without SSL is still required!
{% endhint %}
