---
description: Things to keep in mind before installing Feedbacky.
---

# Prerequisites

{% hint style="warning" %}
<mark style="color:orange;">**Using a Virtual Private Server (VPS)?**</mark>

Make sure that virtualization is supported as it is required for Docker. Additionally if you are limited to less than 2GB of memory you should assign some space for [Swap](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04).
{% endhint %}

*
* An SMTP server is required in order for Feedbacky to send notification to your users to an idea, you can also use one of the following Mail providers;
  * Mailgun
  * SendGrid
* Since Feedbacky is password-less (we use OAuth2), you will need an account on at least one of the following services;
  * [Discord](https://discord.com)
  * [GitHub](https://github.com)
  * [Google](https://www.google.com)

## Compatibility

{% hint style="danger" %}
<mark style="color:red;">**Using a panel software?**</mark>

Please check out our [FAQ section](../../project-overview/frequently-asked-questions.md#can-i-host-feedbacky-on-x-panel) on this.
{% endhint %}

| Operating System | Version                                        | Status | Notes                                 |
| ---------------- | ---------------------------------------------- | :----: | ------------------------------------- |
| **Ubuntu**       | "Focal" 20.04                                  |    ✅   | Installation guide based on Focal.    |
|                  | "Bionic" 18.04                                 |    ✅   |                                       |
| **Debian**       | "Bullseye" 11                                  |    ✅   |                                       |
| **Windows**      | WSL2                                           |    ✅   | _Should work, not tested!_            |
|                  | <p>​Server 2022 </p><p><em>Windows 11</em></p> |   🔧   | Working but not officially supported. |
|                  | <p>​Server 2019</p><p><em>Windows 10</em></p>  |   🔧   | Working but not officially supported. |
| **Mac OS**       |                                                |    ❓   | Unknown and not officially supported. |

## Docker

`Size; ~330MB`

Both the [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) and [Docker Compose](https://docs.docker.com/compose/install/) are necessary dependencies for Feedbacky.

<details>

<summary>Installation Guide</summary>

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

8\. Download the Docker Compose repository.

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

9\. Change the permission of downloaded Docker Compose binaries.

```
sudo chmod +x /usr/local/bin/docker-compose
```

10\. Test that Docker Compose was successfully installed.&#x20;

```
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

```
sudo apt update -y
```

2\. Install the MariaDB server package.

```
sudo apt install -y mariadb-server
```

3\. Run and go through the security script.

```
sudo mysql_secure_installation
```

The script will ask you to set a root password, press `N` to skip it. The root password is already tied to the system on Ubuntu and changing it could result in MariaDB breaking.

### Additional Configuration

Docker will treat your container as a remote machine, we need to change the value of `bind-address`  in order to accept non localhost connections.

4\. Edit your `50-server.cnf`.

```
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

```
sudo systemctl restart mariadb-server
```

</details>

## Webserver

A webserver is needed in order to point a domain to your instance. Any will suffice as long as they support reverse proxy.

We have a guide for both Apache and Nginx, choose the one you prefer.&#x20;

### Apache

Size;

{% hint style="info" %}
Apache will require further configuration to enable reverse proxy support.
{% endhint %}

<details>

<summary>Installation Guide</summary>

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

</details>

### Nginx

Size;

{% hint style="info" %}
Nginx supports reverse proxy out of the box.
{% endhint %}

<details>

<summary>Installation Guide</summary>

1\. Update your system.

```
sudo apt update -y
```

2\. Install the Nginx package.

```
sudo apt install -y nginx
```

3\. Verify that Nginx is running.

```
sudo systemctl status nginx
```

</details>

{% hint style="success" %}
When done, you can proceed to the main installation guide.
{% endhint %}
