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

Each variables already have their own descriptions but we have to go in further details for some.&#x20;

### Networking

| `REACT_APP_SERVER_IP_ADDRESS` | The IP address or domain where Feedbacky will be hosted. |
| ----------------------------- | -------------------------------------------------------- |

{% hint style="danger" %}
Use the correct format or else you won't be able to access your own instance. Let's say our IP address is 188.222.333.22, than the correct format will look like this;

#### Valid

✅ `http://188.222.333.22:8090`

#### Invalid

❌ `http://188.222.333.22:8090/` _<mark style="color:red;">Do not leave a trailing slash</mark> <mark style="color:red;"></mark><mark style="color:red;">**/**</mark> <mark style="color:red;"></mark><mark style="color:red;">at the end.</mark>_

❌ `88.222.333.22:8090` _<mark style="color:red;">The http protocol is missing.</mark>_

❌ `http://188.222.333.22` _<mark style="color:red;">The port is required if you aren't using a domain.</mark>_
{% endhint %}

Follow this guide if you want to use a domain;

{% content-ref url="../using-a-domain.md" %}
[using-a-domain.md](../using-a-domain.md)
{% endcontent-ref %}

### Security

| `JWT_SECRET` | A random generated token used for authentication purposes. |
| ------------ | ---------------------------------------------------------- |

Use this link to generate one yourself;

{% embed url="https://www.grc.com/passwords.htm" %}
You can also use any other JWT secret token generator.
{% endembed %}

{% hint style="success" %}
For extra security, random ASCII characters are recommended. Remember **to not** share your token with anyone else!
{% endhint %}

### Database

| `MYSQL_USERNAME` | The username set during the database setup process.    |
| ---------------- | ------------------------------------------------------ |
| `MYSQL_PASSWORD` | The password set during the database setup process.    |
| `MYSQL_URL`      | Connection information that will be used by Feedbacky. |

### OAuth

Feedbacky is password-less, you will need an account on at least one of the following services;

{% tabs %}
{% tab title="Discord" %}
1\. Go to [https://discord.com/developers/applications](https://discord.com/developers/applications).

2\. Create a new OAuth application.&#x20;

{% embed url="https://cdn.feedbacky.net/static/mp4/discord-oauth-setup.mp4" %}

3\. Create a new redirect with the IP address or domain set [here](installation.md#networking) and include `/auth/discord` at the end.

4\. Fill the necessary variables with your newly created Discord OAuth application.

|   |   |
| - | - |
|   |   |
|   |   |
|   |   |
|   |   |
{% endtab %}

{% tab title="Github" %}
1\. Go to [https://github.com/settings/developers](https://github.com/settings/developers).

2\. Create a new OAuth application.

{% embed url="https://cdn.feedbacky.net/static/mp4/github-oauth-setup.mp4" %}

3\. Fill the **Authorization callback URL** with the IP address or domain set [here](installation.md#networking) and include `/auth/github` at the end.

4\. Fill the necessary variables with your newly created GitHub OAuth application.

|   |                                                                  |
| - | ---------------------------------------------------------------- |
|   | Disabled by default, change to `true` if you want to use GitHub. |
|   |                                                                  |
|   |                                                                  |
|   |                                                                  |
{% endtab %}

{% tab title="Google" %}
{% hint style="danger" %}
The Google OAuth guide is not yet available.
{% endhint %}
{% endtab %}
{% endtabs %}

### Mail



### Extra

## Firewall setup

1\. Since Uncomplicated Firewall (UFW) is installed by default on Ubuntu 20.04, we only need to enable it.

```
sudo ufw enable
```

2\. Finally, forward the Feedbacky server port.&#x20;

```
sudo ufw allow {SERVER_APP_PORT}/tcp
```

| `{SERVER_APP_PORT}` | The server port located in your `.env` file, by default it is 8090. |
| ------------------- | ------------------------------------------------------------------- |

{% hint style="warning" %}
Make sure that you also create a port forwarding rule in your router. If you are using a Virtual Private Server (VPS) check with your provider.
{% endhint %}

## Compiling

You are now ready to compile Feedbacky, with Docker Compose, start a new container.

{% hint style="info" %}
**Running in the background**


{% endhint %}

```
sudo docker-compose up --build
```

This will take a few minutes depending on your hardware capabilities, your CPU usage will also be high during this process.&#x20;

If the compiling was successful, your Feedbacky instance should be ready.
