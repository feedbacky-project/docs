---
description: How to run Feedbacky on your own server.
---

# Getting Started

{% hint style="success" %}
**Are you looking for an affordable hosting provider?**

Feedbacky is in partnership with Senior Hosting to offer you high-performance Virtual Private Servers and Minecraft/Discord bot hosting!

[Check out the offer!](../project-overview/senior-hosting.md)
{% endhint %}

## Choosing an OS

If you can install and use Docker, you should be able to host Feedbacky.

| Operating System | Version                                      | Status | Notes                                 |
| ---------------- | -------------------------------------------- | :----: | ------------------------------------- |
| **Ubuntu**       | "Focal" 20.04                                |    ✅   | Installation guide based on Focal.    |
| ****             | "Bionic" 18.04                               |    ✅   |                                       |
| **Debian**       | "Bullseye" 11                                |    ✅   |                                       |
| **Windows**      | WSL2                                         |    ✅   |                                       |
|                  | Server 2022 _Windows 11_                     |   🔧   | Working but not officially supported. |
|                  | <p>Server 2019</p><p><em>Windows 10</em></p> |   🔧   | Working but not officially supported. |
| **Mac OS**       |                                              |    ❓   | Unknown and not officially supported. |

## Prerequisites

Make sure that you follow the listed prerequisites below.

{% hint style="info" %}
**Installing a prerequisite**

For your convenience, you can expend some prerequisite to follow their installation guide.&#x20;
{% endhint %}

<details>

<summary>Docker with Compose.</summary>

* The [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) and [Docker Compose](https://docs.docker.com/compose/install/) are necessary dependencies for Feedbacky.

### Installing Docker

1\. Make sure that your system is up to date.

```
sudo apt update -y
```

2\. Docker requires some dependencies to be installed on your system.

```
 sudo apt install -y ca-certificates curl gnupg lsb-release
```

3\. Add Docker's official GPG key.

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4\. Use Docker's stable repository.

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5\. Update your system.

```
sudo apt update -y
```

6\. Install Docker Engine and it's related packages.

```
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

7\. Test that the Docker Engine was successfully installed.

```
sudo docker run hello-world
```

### Installing Compose

1\. Download the repository.

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2\. Change the permission of downloaded Docker Compose binaries.

```
sudo chmod +x /usr/local/bin/docker-compose
```

3\. Test that Docker Compose was successfully installed.&#x20;

```
docker-compose --version
```

</details>

<details>

<summary>An SQL database of your choice.</summary>

* It is recommended to use MariaDB instead of MySQL, though it will also work for Feedbacky if you have it installed already.

### Installing MariaDB

1\. Update your system.

```
sudo apt update -y
```

2\. Install the MariaDB server package.

```
sudo apt install -y mariadb-server
```

3\. Go through the guid

</details>

<details>

<summary>A webserver of your choice.</summary>

* Any webserver can be used as long as reverse proxy is supported, below are installation guide for both Apache and NGINX use whichever you want.

### Installing Apache

1\. Update your system.

```
sudo apt update -y
```

2\. Install the Apache package.

```
sudo apt install -y apache2
```

3\. Verify that Apache is running.

```
sudo systemctl status apache2
```

### Installing NGINX

1\. Update your system.

```
sudo apt update -y
```

2\. Install the NGINX package.

```
sudo apt install -y nginx
```

3\. Verify that NGINX is running.

```
sudo systemctl status nginx
```

</details>

<details>

<summary>A firewall of your choice.</summary>

* Port forwarding is required at a later stage.

### Installing Uncomplicated Firewall (UFW)

1\. Update your system.

```
sudo apt update -y
```

2\. Install the Uncomplicated Firewall package.

```
sudo apt install -y ufw
```

&#x20;3\. Enable Uncomplicated Firewall.

```
sudo ufw enable
```

</details>

<details>

<summary>Git tools.</summary>

* You will need to clone the Feedbacky repository.

### Installing Git

1\. Update your system.

```
sudo apt update -y
```

2\. Install the Git package.

```
sudo apt install -y git
```

</details>

* <mark style="background-color:yellow;">Using a Virtual Private Server (VPS) provider? Make sure that the use of virtualization is available.</mark>&#x20;
  * <mark style="background-color:yellow;">Additionally memory limited VPS should have Swap space available. (Have less than 2GB of memory?</mark> [<mark style="background-color:yellow;">Check this tutorial</mark>](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-18-04)<mark style="background-color:yellow;">!)</mark>&#x20;
* You will need an SMTP server or use one of the following Mail providers;
  * Mailgun
  * SendGrid
* Since Feedbacky is password-less (we use OAuth2), you will need an account on at least one of the following services;
  * [Discord](https://discord.com)
  * [GitHub](https://github.com)
  * [Google](https://www.google.com)

## Installation

{% hint style="info" %}
**Want to use the most recent features?**

Consider using our [development branch](../self-hosting-1.0.0/1.0.0-wip-hosting-feedback-instance.md) as it is regularly updated.&#x20;

_Be aware that while many uses this branch in their production servers (in fact_ [_we do!_](https://app.feedbacky.net/b/feedbacky-official)_) using bleeding edge features can lead to some issue._
{% endhint %}

### Cloning the repository

Using `git` clone the official [Feedbacky repository](https://github.com/feedbacky-project/app).

```
git clone https://github.com/feedbacky-project/app
```

### Setting up the database

You need to create an user and a table that will be used for Feedbacky, start the SQL shell.

```
sudo mysql
```

1\. Creating a new user.

{% hint style="danger" %}
Creating a `localhost` user will not work due to the nature of Docker, since it will treat your container as a remote machine.

To mitigate the issue, we will use the `%` wildcard instead.&#x20;
{% endhint %}

```sql
CREATE USER '{MYSQL_USERNAME}'@'%' IDNTIFIED BY '{MYSQL_PASSWORD}';
```

| `{MYSQL_USERNAME}` | The username of your choice. |
| ------------------ | ---------------------------- |
| `{MYSQL_PASSWORD}` | The password of your choice. |

2\. Creating a new table.

```sql
CREATE DATABASE feedbacky;
```

3\. Grant all privileges.

```sql
GRANT ALL PRIVILEGES ON feedbacky.* TO '{MYSQL_USERNAME'@'%';
```

4\. Flush the privileges.

{% hint style="info" %}
Flush will reload the privileges without having the need to restart the MariaDB service.
{% endhint %}

```sql
FLUSH PRIVLEGES;
```

5\. Exiting the SQL shell.

```
exit
```

6\. Additional configuration.

We need to edit the&#x20;



{% hint style="danger" %}
Please note that `localhost` won't work in `MYSQL_URL` variable due to nature of Docker (container is considered as a remote machine).

IP of server must be provided and MySQL must be configured to accept non localhost connections.
{% endhint %}

{% hint style="info" %}
To allow non localhost connections modify `bind-address` at `/etc/mysql/mysql.conf.d/mysqld.cnf` to `0.0.0.0,`restart mysql service with `service mysql restart` and create MySQL user with `%` login access eg. `'feedbacky'@'%'`.
{% endhint %}



\*\*

Due to the nature of Docker, you won't be able to use localhost&#x20;





### Environment Variables

Each variables already have their own descriptions but we have to go in further details with some.&#x20;

Edit your environment.

```
sudo nano .env
```

#### `REACT_APP_SERVER_IP_ADDRESS`

The location where Feedbacky will be hosted, put your machine's local IP address or domain.&#x20;

{% hint style="info" %}
The port is only required if you don't use a domain.
{% endhint %}

{% hint style="danger" %}
**Format**

Make sure you use the correct format for the this variable. For example, let's say our IP address is `188.222.333.22` the format will look like this;

**Correct**

* ✅ http://188.222.333.22:8090

**Invalid**

* ❌ http://188.222.333.22:8090<mark style="color:red;">**/**</mark> - _Do not use trailing slash at the end_
* ​❌ <mark style="color:red;">**http://**</mark>188.222.333.22:8090 - _`http://` is missing_
* ​❌ http://188.222.333.22<mark style="color:red;">**:8090**</mark> - _The port is missing_
{% endhint %}



{% code title=".env" %}
```bash
# Domain or IP address where Feedbacky is hosted, CLIENT_APP_PORT is required here if you don't use port 80 (web port) or domain
REACT_APP_SERVER_IP_ADDRESS=http://example.com
# Name of your self hosted Feedbacky service
REACT_APP_SERVICE_NAME=Feedbacky
# Link to default user avatar image, use %nick% placeholder to replace with User nickname
REACT_APP_DEFAULT_USER_AVATAR=https://static.plajer.xyz/avatar/generator.php?name=%nick%

# Port where client application will run, if this port is used replace it with different one
CLIENT_APP_PORT=8090
# Port where server application will run, if this port is used replace it with different one
SERVER_APP_PORT=8095

# Secret token for authentication purposes, you can generate one here https://www.grc.com/passwords.htm
JWT_SECRET=secretPass

# Database credentials
MYSQL_USERNAME=username
MYSQL_PASSWORD=passwd
# Replace <ip_address> <port> and <database_name> with proper values
MYSQL_URL=jdbc:mysql://<ip_address>:<port>/<database_name>?useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=UTF-8

# Feedbacky is passwordless and relies on 3rd party providers to log in, please enable at least 1 of providers below.
# Discord (https://discordapp.com/) login integration enabled
# To create OAuth application go here https://discordapp.com/developers/applications
OAUTH_DISCORD_ENABLED=false
OAUTH_DISCORD_REDIRECT_URI=redirectUri
OAUTH_DISCORD_CLIENT_ID=clientId
OAUTH_DISCORD_CLIENT_SECRET=clientSecret

# GitHub (https://github.com) login integration enabled
# To create OAuth application go here https://github.com/settings/developers
OAUTH_GITHUB_ENABLED=false
OAUTH_GITHUB_REDIRECT_URI=redirectUri
OAUTH_GITHUB_CLIENT_ID=clientId
OAUTH_GITHUB_CLIENT_SECRET=clientSecret

# Google (https://accounts.google.com) login integration enabled
# To create OAuth application go here https://console.developers.google.com/apis/dashboard create new project and check Credentials section
OAUTH_GOOGLE_ENABLED=false
OAUTH_GOOGLE_REDIRECT_URI=redirectUri
OAUTH_GOOGLE_CLIENT_ID=clientId
OAUTH_GOOGLE_CLIENT_SECRET=clientSecret

# Name of mail sender
MAIL_SENDER=Feedbacky <no-reply@feedbacky.net>
# Currently available mail services:
# * mailgun - https://www.mailgun.com/ (credit card required to set up account) (5k mails for 3 months, then $0.80 per 1k mails)
# * sendgrid - https://sendgrid.com/ (100 mails per day)
# * smtp - your own SMTP server for sending mails
MAIL_SERVICE_TYPE=mailgun
# API key provided by mailgun service
MAIL_MAILGUN_API_KEY=apiKey
# API url provided by mailgun, should be something like https://api.mailgun.net/<version>/<domain>/messages
MAIL_MAILGUN_API_BASE_URL=baseUrl
# API key provided by sendgrid service
MAIL_SENDGRID_API_KEY=apiKey
# API url provided by sendgrid, should be something like https://api.sendgrid.com/v3/mail/send
MAIL_SENDGRID_API_BASE_URL=baseUrl
# SMTP server credentials
MAIL_SMTP_USERNAME=username
MAIL_SMTP_PASSWORD=passwd
MAIL_SMTP_HOST=host.com
MAIL_SMTP_PORT=587

# Is image compression for images enabled? If yes credentials below must be valid.
IMAGE_COMPRESSION_ENABLED=false
# Currently available compressors:
# * cheetaho - https://cheetaho.com/ (free 500 compressions per month)
IMAGE_COMPRESSION_TYPE=cheetaho
# API key from https://cheetaho.com/ service
IMAGE_COMPRESSION_CHEETAHO_API_KEY=apiKey

# Allow users to post comments to closed ideas. By default value is false.
SETTINGS_ALLOW_COMMENTING_CLOSED_IDEAS=false
```
{% endcode %}



* App IP
* Mail types
* OAuth2



* Setup firewall
* Creating docker-compose&#x20;

## Compiling

*

Running Feedbacky in background.

* tmux or screen
* docker logs

##
