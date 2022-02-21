---
description: Use your own domain with Feedbacky.
---

# Domain Setup

## Record

Before going into details, you must have your server pointed to your domain.

1\. Access the DNS settings of your domain provider.

2\. Create an **A Record**.

3\. Find your public IP address.

```bash
curl ipinfo.io/ip
```

4\. Put your public IP address in the address field.

## Port Forwarding

Your webserver also needs to be accessible over the internet.&#x20;

1\. Forward port `80`.&#x20;

```bash
sudo ufw allow 80/tcp
```

2\. Forward port `443`.

```bash
sudo ufw allow 443/tcp
```

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

3\. Add the following content to your newly created Virtual Host file.

{% tabs %}
{% tab title="Apache" %}
{% code title="{feedbacky.yourdomain}.conf" %}
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
{% endtab %}

{% tab title="Nginx" %}
{% code title="{feedbacky.yourdomain}.conf" %}
```nginx
server {
    listen 80;
    server_name {DOMAIN_NAME};

     location / {
       proxy_pass {SERVER_IP}:{CLIENT_APP_PORT};
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

| `{DOMAIN_NAME}`     | Your fully qualified domain domain, example; **feedbacky.app.cool**.                    |
| ------------------- | --------------------------------------------------------------------------------------- |
| `{SERVER_IP}`       | The IP address (not public) of your Virtual Private Server (VPS) or dedicated hardware. |
| `{CLIENT_APP_PORT}` | The client port located in your `.env` file, by default it is 8090.                     |

4\. Save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.

### Additional steps

{% tabs %}
{% tab title="Apache" %}
5\. By default proxy is disabled in Apache, enable it.

```bash
sudo a2enmod proxy
```

****

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

Each time you will edit your Virtual Host located in `/etc/nginx/sites-available` it will also update the one created in `/etc/nginx/sites-enabled`.&#x20;
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

## Certificate

### Generating

### Updating Virtual Host

### Additional Steps

### Renewing

## SSL Alternative

An alternative method that you can follow instead of manually generating a certificate and adding additional values to your VHost is by using Cloudflare.

In short by adding your website to Cloudflare you will be able to forward your board with Cloudflare's own proxy.

You can read more [here](https://support.cloudflare.com/hc/en-us/articles/201720164).





####



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
