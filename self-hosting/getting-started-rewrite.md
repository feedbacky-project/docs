---
description: How to run Feedbacky on your own server.
---

# Getting Started (Rewrite)

{% hint style="success" %}
**Are you looking for an affordable hosting provider?**

Feedbacky is in partnership with Senior Hosting to offer you high-performance Virtual Private Servers and Minecraft/Discord bot hosting!

[Check out the offer!](../project-overview/senior-hosting.md)
{% endhint %}

{% hint style="info" %}
**Do you want to use the most recent features?**

If so, consider using our [development branch](https://github.com/feedbacky-project/app/tree/development) as it is regularly updated. Be aware that while many uses this branch in their production servers (in fact [we do!](https://app.feedbacky.net/b/feedbacky-official)) using bleeding edge features can lead to some instability.
{% endhint %}

This installation covers all the necessary tools you will need, if you already have all of the necessary dependencies checkout our abridged guide.

## Prerequisites

Before you can start self-hosting Feedbacky make sure that you follow the prerequisites, we will cover the necessary installation of them below.

### Compatibility

|   |   |   |
| - | - | - |
|   |   |   |
|   |   |   |
|   |   |   |

* Docker with Compose.
* MySQL or MariaDB (recommended over MySQL) database.
* Apache or NGINX web server.

{% hint style="info" %}
**Using a Virtual Private Server (VPS) provider?**

Make sure that the use of virtualization is available. Additionally memory limited VPS should have Swap space available. (Have less than 2GB of memory? [Check this tutorial](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-18-04)!)&#x20;
{% endhint %}

* You will need an SMTP server or use one of the following Mail providers;
  * Mailgun
  * SendGrid
* Since Feedbacky is password-less (we use OAuth2), you will need an account on at least one of the following services;
  * [Discord](https://discord.com)
  * [GitHub](https://github.com)
  * [Google](https://www.google.com)

## Installing

### Docker

Simple installation guide for Docker with Compose, ultimately the maintenance of your Docker install is up to you, ditch this guide and use the official Docker documentation if you encounter any issues.

{% hint style="info" %}
The methods shown below are taken directly from Docker's own documentation, you can read more about Docker Engine [here](https://docs.docker.com/engine/install/ubuntu/) and Docker Compose [here](https://docs.docker.com/compose/install/).
{% endhint %}

Make sure that your system is up to date.

```
sudo apt update -y && sudo apt upgrade -y
```

Docker requires some dependencies to be installed on your system.

```
 sudo apt install -y ca-certificates curl gnupg lsb-release
```

Add Docker's official GPG key.

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Use Docker's stable directory.

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update your system for the repository to work.

```
sudo apt update -y
```

Install the Docker Engine.

```
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

{% hint style="success" %}
**Testing**

Test that Docker was successfully installed.

```
 sudo docker run hello-world
```
{% endhint %}

### Docker Compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Change the permission of downloaded Docker Compose binaries.

```
sudo chmod +x /usr/local/bin/docker-compose
```

{% hint style="success" %}
**Testing**

Test that Docker Compose was successfully installed.&#x20;

```
docker-compose --version
```
{% endhint %}

### MariaDB



### Choosing a Webserver

### Installing Feedbacky

* Cloning repo
* Configuring env var
* docker-compose&#x20;

## Configuring

* App IP
* Mail types
* OAuth2



Running Feedbacky in background.

* tmux or screen
* docker logs

##
