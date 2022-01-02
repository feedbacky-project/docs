# Reverse Proxy with NGINX
NGINX is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache. The software was created by Igor Sysoev and publicly released in 2004. Nginx is free and open-source software, released under the terms of the 2-clause BSD license. A large fraction of web servers use NGINX, often as a load balancer.

(*Source [Wikipedia](https://en.wikipedia.org/wiki/Nginx)*)

More info at https://nginx.org/

### Installing
Before we proceed installing the NGINX package, make sure you that everything is up to date:

```text
sudo apt update -y && sudo apt upgrade -y
```

Next, we'll install Apache:

```text
sudo apt install nginx -y
```

### Creating a new VirtualHost

VirtualHosts allows you to run multiple website on a single webserver (website1.com, website2.com, etc..) this is useful especially if you are also hosting other web services such as a website or store front.

Access NGINX's VirtualHost directory:

```text
cd /etc/nginx/sites-available
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
server {
      listen 80;
      server_name {DOMAIN_NAME};

      location / {
        proxy_pass http://{SERVER_IP}:{CLIENT_APP_PORT}:
      }
}
```

- **`{DOMAIN_NAME}`**: Your fully qualified domain name, for example `feedbacky.app.cool`.
- **`{SERVER_IP}`**: The public IP address used by your hosting machine.
- **`{CLIENT_APP_PORT}`**: The client port located in your `.env` file, by default it is 8090.

Save the file with `CTRL` + `S`.

Exit nano with `CTRL` + `C`.

### Creating a symlink

A symlink or symbolic link, is a shortcut to another file. Each time you will edit your VirtualHost in `/etc/nginx/sites-available` it will also update the one created in `/etc/nginx/sites-enabled`.

```text
sudo ln -s /etc/nginx/sites-available/{DOMAIN_NAME}.conf /etc/nginx/sites-enabled/{DOMAIN_NAME}.conf
```
### Enabling the VirtualHost

To enable your VirtualHost we must reload NGINX:

```text
sudo service nginx reload
```

Finally, using the DNS provider of your choice, create an **A Record** that points to your machine's public IP adress and Apache will handle the for rest for you.

Voila 🎉, you just created a VirtualHost with NGINX for your Feedbacky instance!
