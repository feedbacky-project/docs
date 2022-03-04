---
description: Self-hosting your own Feedbacky instance.
---

# Installation

{% hint style="success" %}
<mark style="color:green;">**Looking for an affordable hosting provider?**</mark>

We've partnered with Senior Hosting to offer you affordable, high-performance VPS with Minecraft/Discord bot hosting.\
**Get 10% off using code `FEEDBACKY`!**

[Check out the offer!](https://billing.senior-host.com/link.php?id=1)
{% endhint %}

Thank you for your interest in using Feedbacky, follow this guide step by step in order to self-host your own instance, we have made this guide beginner friendly specifically for Linux newbies.

Please consider [donating](../../project-overview/donating.md) to Feedbacky!

## Prerequisites

* Feedbacky will consume approximately 300MB of memory when running.
* At least 200MB of free space is needed for Feedbacky itself.
* Feedbacky is passwordless (we use OAuth), you will need an account with at least one of these services;
  * [Discord](https://discord.com)
  * [GitHub](https://github.com)
  * [Google](https://www.google.com)
  * [GitLab](https://about.gitlab.com)
* In order for your users to be notified when their subscribed content is updated, you will need an SMTP server or use one of the following mail providers;
  * [Mailgun](https://www.mailgun.com)
  * [SendGrid](https://sendgrid.com)

### Operating System

{% hint style="danger" %}
<mark style="color:red;">**Heads up!**</mark>

If you plan on using a panel with Feedbacky, please read this [FAQ section](../../project-overview/faq.md#can-i-host-feedbacky-on-x-panel).
{% endhint %}

| Operating System | Version                                       | Status | Notes                                 |
| ---------------- | --------------------------------------------- | :----: | ------------------------------------- |
| **Ubuntu**       | "Focal" 20.04                                 |    ✅   | Installation guide based on Focal.    |
|                  | "Bionic" 18.04                                |    ✅   |                                       |
| **Debian**       | "Bullseye" 11                                 |    ✅   |                                       |
| **Windows**      | WSL2                                          |    ✅   | _Should work, not tested!_            |
|                  | <p>​Server 2022</p><p><em>Windows 10</em></p> |   🔧   | Working but not officially supported. |
|                  | <p>​Server 2019</p><p><em>Windows 10</em></p> |   🔧   | Working but not officially supported. |
| **Mac OS**       |                                               |    ❓   | Unknown and not officially supported. |

## Docker

`Size; ~330MB`

Both the [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) and [Docker Compose](https://docs.docker.com/compose/install/) are necessary dependencies for Feedbacky.

<details>

<summary>Installation Guide</summary>

1\. Make sure that your system is up to date.

```bash
sudo apt update -y
```

2\. Docker requires some dependencies to be installed on your system.

```bash
 sudo apt install -y ca-certificates curl gnupg lsb-release
```

3\. Add Docker's official GPG key.

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4\. Use Docker's stable repository.

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5\. Update your system.

```bash
sudo apt update -y
```

6\. Install Docker Engine and it's related packages.

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

7\. Test that the Docker Engine was successfully installed.

```bash
sudo docker run hello-world
```

8\. Download the Docker Compose repository.

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

9\. Change the permission of downloaded Docker Compose binaries.

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

10\. Test that Docker Compose was successfully installed.

```bash
docker-compose --version
```

</details>

## MariaDB

`Size; ~200MB`

MariaDB is recommended over MySQL as it brings better performance but MySQL will still work with Feedbacky if you already have it installed.

{% hint style="danger" %}
Additional configuration needed, **do not** skip this part if you already have MariaDB installed.
{% endhint %}

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the MariaDB server package.

```bash
sudo apt install -y mariadb-server
```

3\. Run and go through the security script.

```bash
sudo mysql_secure_installation
```

The script will ask you to set a root password, press `N` to skip it. The root password is already tied to the system on Ubuntu and changing it could result in MariaDB breaking.

**Additional Configuration**

Docker will treat your container as a remote machine, we need to change the value of `bind-address` in order to accept non localhost connections.

4\. Edit your `50-server.cnf`.

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

5\. Change the value of `bind-address` to `0.0.0.0`.

{% code title="50-server.cnf" %}
```
bind-address            = 0.0.0.0
```
{% endcode %}

6\. Save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.

7\. Restart MariaDB.

```bash
sudo systemctl restart mariadb-server
```

</details>

## Webserver

A webserver is needed in order to point a domain to your instance. Any will suffice as long as they support reverse proxy.

We have a guide for both Apache and Nginx, choose the one you prefer.

### Apache

`Size; ~10MB`

{% hint style="info" %}
Apache will require further configuration to enable reverse proxy support.
{% endhint %}

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the Apache package.

```bash
sudo apt install -y apache2
```

3\. Verify that Apache is running.

```bash
sudo systemctl status apache2
```

</details>

### Nginx

`Size; ~2MB`

{% hint style="info" %}
Nginx supports reverse proxy out of the box.
{% endhint %}

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the Nginx package.

```bash
sudo apt install -y nginx
```

3\. Verify that Nginx is running.

```bash
sudo systemctl status nginx
```

</details>

### Caddy

{% hint style="info" %}
Coming soon!
{% endhint %}

## Certbot

`Size; ~2MB`

Certbot is a tool to generate SSL certificates, which we are covering in our [Domain Setup](../domain-setup.md#ssl) guide.

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the certbot package.

```bash
sudo apt install -y certbot
```

3\. Install the dependencies for your webserver.

**For Apache**;

```bash
sudo apt install -y python3-certbot-apache
```

**For Nginx**;

```bash
sudo apt install -y python3-certbot-nginx
```

</details>

## Terminal Multiplexer

_**Optional**_

During the startup process Feedbacky will use your current terminal window and unless you SSH again in your server with a new session, you won't be able to do anything else.

The solution to this is by using a terminal multiplexer, allowing you to run multiple sessions under a single window.

### tmux

`Size; ~940KB`

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the tmux package.

```bash
sudo apt install -y tmux
```

</details>

### Screen

`Size; ~840KB`

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```bash
sudo apt update -y
```

2\. Install the Screen package.

```bash
sudo apt install -y screen
```

</details>
