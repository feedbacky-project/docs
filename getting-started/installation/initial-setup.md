---
description: >-
  Downloading the necessary files for Feedbacky itself, setting up the database
  and firewall.
---

# Initial Setup

## Necessary files

1\. Create a new directory named "feedbacky" in the `/etc` directory and access it.

```bash
sudo mkdir /etc/feedbacky && cd /etc/feedbacky
```

2\. Run the command below to download the necessary files.

```bash
sudo curl -O "https://raw.githubusercontent.com/feedbacky-project/app/master/{.env,docker-compose.yml}"
```

* `.env` is the file containing the Feedbacky variable in which you can configure to your needs.
* `docker-compose.yml` is the instruction file for Docker.

## Database setup

1\. Start the SQL shell.

```bash
sudo mysql
```

2\. Create a new user for Feedbacky.

{% hint style="info" %}
As stated [previously](./#mariadb), we won't be able to create a `localhost` user, instead we'll use the `%` wildcard instead.
{% endhint %}

```sql
CREATE USER '{MYSQL_USERNAME}'@'%' IDENTIFIED BY '{MYSQL_PASSWORD}';
```

| `{MYSQL_USERNAME}` | The username of your choice. |
| ------------------ | ---------------------------- |
| `{MYSQL_PASSWORD}` | The password of your choice. |

3\. Create a new table for Feedbacky.

```sql
CREATE DATABASE feedbacky;
```

4\. Grant all privileges to your MySQL user.

```sql
GRANT ALL PRIVILEGES ON feedbacky.* TO '{MYSQL_USERNAME}'@'%';
```

5\. Flush the privileges.

```sql
FLUSH PRIVLEGES;
```

{% hint style="info" %}
Flush will reload the privileges without having the need to restart the MariaDB daemon.
{% endhint %}

6\. Exit the SQL shell.

```
exit
```

## Firewall setup

Uncomplicated Firewall (UFW) is the default Firewall on Ubuntu 20.04, it is disabled by default.

1\. Enable Uncomplicated Firewall (UFW).

```bash
sudo ufw enable
```

2\. Forward the Feedbacky server port.&#x20;

```bash
sudo ufw allow 8095/tcp
```

{% hint style="info" %}
If you plan on using a different port, make sure that you change the value of the `SERVER_APP_PORT` variable when [configuring](configuring.md)!
{% endhint %}

{% hint style="success" %}
Make sure that you also create a port forwarding rule in your router settings page. If you are using a Virtual Private Server (VPS) check with your provider.
{% endhint %}
