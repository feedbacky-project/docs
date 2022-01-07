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

#### 1. Make sure that your system is up to date.

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

* It is recommended to use MariaDB over MySQL as the former brings performance advantages, though MySQL will still work for Feedbacky if you prefer it.

### Installing MariaDB

1\. Update your system.

```
sudo apt update -y
```

2\. Install the MariaDB server package.

```
sudo apt install -y mariadb-server
```

#### 3. Run and go through the security script.

```
sudo mysql_secure_installation
```

During the script it will ask you to set a root password, press `N` to skip it.&#x20;

On Ubuntu the root password is already tied to the system and changing it could result in MariaDB breaking.

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

{% hint style="warning" %}
Docker will treat your container as a remote machine, we need to change the `bind-address` setting in order to accept non localhost connections.
{% endhint %}

1\. Edit your `50-server.cnf`.

```
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

2\. Change the value of `bind-address` to `0.0.0.0`.

{% code title="50-server.cnf" %}
```
bind-address = 0.0.0.0
```
{% endcode %}

3\. You can now save the file with `CTRL` + `S` and exit nano with `CTRL` + `C`.

4\. Restart MariaDB.

```
sudo systemctl restart mariadb-server
```

Next you'll need to create a new user and a table that Feedbacky will use.

6\. Start the SQL shell.

```
sudo mysql
```

7\. Create a new user.

{% hint style="danger" %}
As stated above we won't be able to create a `localhost` user.

To mitigate that, use the `%` wildcard instead.
{% endhint %}

```sql
CREATE USER '{MYSQL_USERNAME}'@'%' IDNTIFIED BY '{MYSQL_PASSWORD}';
```

| `{MYSQL_USERNAME}` | The username of your choice. |
| ------------------ | ---------------------------- |
| `{MYSQL_PASSWORD}` | The password of your choice. |

8\. Creating a new table.

```sql
CREATE DATABASE feedbacky;
```

9\. Grant all privileges.

```sql
GRANT ALL PRIVILEGES ON feedbacky.* TO '{MYSQL_USERNAME'@'%';
```

10\. Flush the privileges.

{% hint style="info" %}
Flush will reload the privileges without having the need to restart the MariaDB service.
{% endhint %}

```sql
FLUSH PRIVLEGES;
```

11\. Exiting the SQL shell.

```
exit
```

### Environment Variables

&#x20;

## Configuring

Before we can compile and run Feedbacky, you need to edit some environment variables.

1\. Edit the `.env` file.&#x20;

```
sudo nano .env
```



#### `REACT_APP_SERVER_IP_ADDRESS`

The location where Feedbacky will be hosted, put your machine's local IP address or domain.&#x20;

{% hint style="info" %}
The port is only required if you don't use a domain.
{% endhint %}

{% hint style="danger" %}
Make sure you use the correct format for the this variable. For example, let's say our IP address is `188.222.333.22` the format will look like this;

**Correct**

* ✅ http://188.222.333.22:8090

**Invalid**

* ❌ http://188.222.333.22:8090<mark style="color:red;">**/**</mark> - _Do not leave a trailing slash at the end_
* ​❌ <mark style="color:red;">**http://**</mark>188.222.333.22:8090 - _`http://` is missing_
* ​❌ http://188.222.333.22<mark style="color:red;">**:8090**</mark> - _The port is missing_
{% endhint %}

### Security



### Database



### OAuth2



### Mail



### Extra













Each variables already have their own descriptions but we have to go in further details with some.&#x20;







* Setup firewall

## Compiling

*

Running Feedbacky in background.

* tmux or screen
* docker logs

##
