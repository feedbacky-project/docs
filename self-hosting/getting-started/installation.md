---
description: The main installation process.
---

# Installation

## Database setup

1\. Start the SQL shell.

```
sudo mysql
```

2\. Create a new user

{% hint style="danger" %}
As stated [previously](prerequisites.md#extra-configuration), we won't be able to create a `localhost` user, instead use the `%` wildcard instead.
{% endhint %}

```sql
CREATE USER '{MYSQL_USERNAME}'@'%' IDNTIFIED BY '{MYSQL_PASSWORD}';
```

| `{MYSQL_USERNAME}` | The username of your choice. |
| ------------------ | ---------------------------- |
| `{MYSQL_PASSWORD}` | The password of your choice. |

3\. Create a new table

```sql
CREATE DATABASE feedbacky;
```

4\. Grant all privileges.

```sql
GRANT ALL PRIVILEGES ON feedbacky.* TO '{MYSQL_USERNAME}'@'%';
```

5\. Flush the privileges.

```sql
FLUSH PRIVLEGES;
```

{% hint style="info" %}
Flush will reload the privileges without having the need to restart the MariaDB service.
{% endhint %}

6\. Exit the SQL shell.

```
exit
```

## Cloning the repository

Using `git`, clone the official [Feedbacky repository](https://github.com/feedbacky-project/app).

{% hint style="info" %}
**Want to use the most recent features?**

Consider using our [development branch](../../self-hosting-1.0.0/1.0.0-wip-hosting-feedback-instance.md) as it is regularly updated.&#x20;

_Be aware that while many uses this branch in their production servers (in fact_ [_we do!_](https://app.feedbacky.net/b/feedbacky-official)_) using bleeding edge features can lead to some issues._
{% endhint %}

```
sudo git clone https://github.com/feedbacky-project/app /opt/feedbacky && cd /opt/feedbacky
```

## Configuring

Edit the environment variable file.

```
sudo nano .env
```

Each variables already have their own descriptions but we have to go in further details with some.&#x20;

### Networking

{% tabs %}
{% tab title="REACT_APP_SERVER_IP_ADDRESS" %}
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
{% endtab %}
{% endtabs %}

### Security

{% tabs %}
{% tab title="JWT_SECRET" %}
A random generated token used for authentication purposes, you can generate one yourself below.



{% embed url="https://www.grc.com/passwords.htm" %}
You can also use any other JWT secret token generator.
{% endembed %}



{% hint style="success" %}
Random ASCII character recommended!
{% endhint %}
{% endtab %}
{% endtabs %}

### OAuth2

{% hint style="info" %}
Feedbacky is password-less, you will need an account on at least one of the following services;
{% endhint %}

{% tabs %}
{% tab title="Discord" %}
| `OAUTH_DISCORD_ENABLED`      | Default value is `false`.                                       |
| ---------------------------- | --------------------------------------------------------------- |
| `OAUTH_DISCORD_REDIRECT_URI` |                                                                 |
| `OAUTH_DISCORD_CLIENT_ID`    |                                                                 |
| `OAUTH_DISCORD_SECRET`       |                                                                 |
{% endtab %}

{% tab title="Github" %}

{% endtab %}

{% tab title="Google" %}

{% endtab %}
{% endtabs %}





## Firewall setup

1\. Since Uncomplicated Firewall (UFW) is installed by default on Ubuntu 20.04, we only need to enable it.

```
sudo ufw enable
```

2\. Finally, forward the Feedbacky server port.&#x20;

```
sudo ufw allow {REACT_APP_SERVER_PORT}/tcp
```

## Compiling

{% hint style="info" %}
Running in background.
{% endhint %}
