---
description: Necessary packages in order for Feedbacky to work
---

# Dependencies

{% hint style="warning" %}
Make sure that you have all the necessary packages installed, Feedbacky will not properly work if not.
{% endhint %}

## Docker

> At least \~350Mb

Docker will run the infrastructure necessary for Feedbacky to work.

### Installing <mark style="color:green;">↴</mark>

1\. Update your system.

{% hint style="success" %}
Before installing any package, it is good practice to always update your system.
{% endhint %}

```bash
sudo apt-get update -y
```

2\. Download the necessary dependencies.

```bash
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

3\. Add the Docker GPG key.

```bash
sudo mkdir -p /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

4\. Setup the Docker repository.

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5\. Update your system once again.

```bash
sudo apt-get update -y
```

6\. Install the Docker Engine (Docker Compose is included).

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## MariaDB

> At least \~200Mb

A database is needed to store important data, we recommend MariaDB over MySQL since it brings better performance, though MySQL will work just fine if you already have it installed.

{% hint style="danger" %}
Additional configuration needed, **do not** skip this part if you already have MariaDB installed.
{% endhint %}

### Installing <mark style="color:green;">↴</mark>

1\. Update your system.

```bash
sudo apt-get update -y
```

2\. Install the MariaDB server package.

```bash
sudo apt-get install -y mariadb-server
```

3\. Run and go through the security script.

```bash
sudo mysql_secure_installation
```

The script will ask you to set a root password, press `N` to skip it. The root password is already tied to the system on Ubuntu and changing it could result in MariaDB breaking.

### **Additional Configuration** <mark style="color:red;">⚠</mark>

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
sudo systemctl restart mariadb
```

## Nginx

> At least \~2Mb

A webserver is required in order to point a domain to your instance, we recommend that you use Nginx but please note that any other webserver will work as long as it supports reverse proxy.

### Installing <mark style="color:green;">↴</mark>

1\. Update your system.

```bash
sudo apt-get update -y
```

2\. Install the UFW package.

```bash
sudo apt-get install -y nginx
```

3\. Verify that UFW is working.

```bash
sudo systemctl status nginx
```

## Additional Software

{% hint style="info" %}
Any additional software listed below is only required depending on your Linux distribution, you can skip this step if it doesn't relate to you.
{% endhint %}

### Uncomplicated Firewall (UFW)

Comes by default with Ubuntu, Debian users are required to install manually.

1\. Update your system.

```bash
sudo apt-get update -y
```

2\. Install the UFW package.

```bash
sudo apt-get install -y ufw
```

3\. Verify that UFW is working.

```bash
sudo systemctl status ufw
```
