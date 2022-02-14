---
description: Initial setup, database configuration and editing the environment variables.
---

# Installation

## Initial setup

1\. Create a new directory named "feedbacky" in the `/etc` directory and access it.

```
sudo mkdir /etc/feedbacky && cd /etc/feedbacky
```

2\. Run the command below to download the necessary files.

```
sudo curl -O "https://raw.githubusercontent.com/feedbacky-project/app/master/{.env,docker-compose.yml}"
```

## Database setup

1\. Start the SQL shell.

```
sudo mysql
```

2\. Create a new user for Feedbacky.

{% hint style="info" %}
As stated [previously](prerequisites.md#extra-configuration), we won't be able to create a `localhost` user, instead we'll use the `%` wildcard instead.
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

## Configuring

Environment variables must be edited in order for your instance to properly work.

{% hint style="info" %}
Each variables already have their own descriptions but we will go into further details for some.&#x20;
{% endhint %}

1\. Edit the `.env` file.

```
sudo nano .env
```

### Networking

| `REACT_APP_SERVER_IP_ADDRESS` | The IP address or domain where Feedbacky will be hosted. |
| ----------------------------- | -------------------------------------------------------- |
| `CLIENT_APP_PORT`             | Port for the client application, by default it is 8090.  |
| `SERVER_APP_PORT`             | Port for the server application, by default it is 8095.  |

Follow this guide if you want to use a domain;

{% content-ref url="../domain-setup.md" %}
[domain-setup.md](../domain-setup.md)
{% endcontent-ref %}

{% hint style="info" %}
<mark style="color:blue;">**Address Format**</mark>

Using an invalid IP address format will stop you from accessing your own instance.&#x20;

Let's say that our IP address is `188.222.333.22`, the valid format will look like this; ``&#x20;

****

{% hint style="danger" %}
#### <mark style="color:red;">**Invalid**</mark>

_Do not leave a trailing (**/**) slash at the end;_

`http://188.222.333.22:8090/`&#x20;
{% endhint %}

{% hint style="danger" %}
#### <mark style="color:red;">**Invalid**</mark>

_The protocol (**http**) is missing;_

`88.222.333.22:8090`
{% endhint %}

{% hint style="danger" %}
#### <mark style="color:red;">**Invalid**</mark>

_The port is missing (can be skipped if you use a domain);_

`http://188.222.333.22`
{% endhint %}



{% hint style="success" %}
#### <mark style="color:green;">**Valid**</mark>

_The protocol and port are present with no trailing slash;_

`http://188.222.333.22:8090`
{% endhint %}
{% endhint %}

### Security

| `JWT_SECRET` | A random generated token used for authentication purposes. |
| ------------ | ---------------------------------------------------------- |

{% tabs %}
{% tab title="GRC" %}
Use this link to generate one yourself;

{% embed url="https://www.grc.com/passwords.htm" %}

Any other JWT secret token generation can also be used.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
For extra security, random ASCII characters are recommended. Remember, **do not share** your token with anyone else!
{% endhint %}

### Database

| `MYSQL_USERNAME` | The username set during the database setup process.    |
| ---------------- | ------------------------------------------------------ |
| `MYSQL_PASSWORD` | The password set during the database setup process.    |
| `MYSQL_URL`      | Connection information that will be used by Feedbacky. |

### OAuth

{% tabs %}
{% tab title="Discord" %}
Follow these steps to use the Discord OAuth.

1\. Access Discord's [Developer Portal](https://discord.com/developers/applications).

2\. Create a new OAuth application.&#x20;

{% embed url="https://cdn.feedbacky.net/static/mp4/discord-oauth-setup.mp4" %}

3\. Add a new redirect with the IP address or domain set [here](installation.md#networking) and include `/auth/discord` at the end.

4\. Fill the necessary variables with your newly created Discord OAuth application.

| `OAUTH_DISCORD_ENABLED`      | Disabled by default, to use Discord OAuth change to `true`.    |
| ---------------------------- | -------------------------------------------------------------- |
| `OAUTH_DISCORD_REDIRECT_URI` | Your instance domain with `/auth/discord` included at the end. |
| `OAUTH_DISCORD_CLIENT_ID`    | Your OAuth client ID.                                          |
| `OAUTH_DISCORD_SECRET`       | Your OAuth client secret.                                      |
{% endtab %}

{% tab title="Github" %}
Follow these steps to use the GitHub OAuth.

1\. Access GitHub's [Developer settings](https://github.com/settings/developers).

2\. Create a new OAuth application.

{% embed url="https://cdn.feedbacky.net/static/mp4/github-oauth-setup.mp4" %}

3\. Fill the **Authorization callback URL** with the IP address or domain set [here](installation.md#networking) and include `/auth/github` at the end.

4\. Fill the necessary variables with your newly created GitHub OAuth application.

| `OAUTH_GITHUB_ENABLED`       | Disabled by default, to use GitHub OAuth change to `true`.    |
| ---------------------------- | ------------------------------------------------------------- |
| `OAUTH_GITHUB_REDIRECT_URI`  | Your instance domain with `/auth/github` included at the end. |
| `OAUTH_GITHUB_CLIENT_ID`     | Your OAuth client ID.                                         |
| `OAUTH_GITHUB_CLIENT_SECRET` | Your OAuth client secret.                                     |
{% endtab %}

{% tab title="Google" %}
{% hint style="danger" %}
The Google OAuth guide is not yet available.
{% endhint %}
{% endtab %}

{% tab title="GitLab" %}
Follow these steps to use the GitLab OAuth.

1\. Access GitLab's [Developer Portal](https://gitlab.com/-/profile/applications).

2\. Create a new OAuth application.&#x20;

{% embed url="https://cdn.feedbacky.net/static/mp4/discord-oauth-setup.mp4" %}

3\. Add a new redirect with the IP address or domain set [here](installation.md#networking) and include `/auth/gitlab` at the end.

4\. Fill the necessary variables with your newly created Discord OAuth application.

| `OAUTH_GITLAB_ENABLED`      | Disabled by default, to use GitLab OAuth change to `true`.    |
| --------------------------- | ------------------------------------------------------------- |
| `OAUTH_GITLAB_REDIRECT_URI` | Your instance domain with `/auth/gitlab` included at the end. |
| `OAUTH_GITLAB_CLIENT_ID`    | Your OAuth client ID.                                         |
| `OAUTH_GITLAB_SECRET`       | Your OAuth client secret.                                     |
{% endtab %}
{% endtabs %}

### Mail

| `MAIL_SERVICE_TYPE` | Choose one of the options below. |
| ------------------- | -------------------------------- |

{% tabs %}
{% tab title="SMTP" %}
| `MAIL_SMTP_USERNAME` | Your username.                         |
| -------------------- | -------------------------------------- |
| `MAIL_SMTP_PASSWORD` | The password.                          |
| `MAIL_SMTP_HOST`     | A reachable domain or IP address.      |
| `MAIL_SMTP_PORT`     | A reachable port, by default it is 25. |
{% endtab %}

{% tab title="Mailgun" %}
[Mailgun](https://www.mailgun.com) is a mail provider supported by Feedbacky.

{% hint style="info" %}
Send up to 5000 mails during a _3 month trial_ than _$0.80_ per 1000 mails.

A credit card is also required for sign up.
{% endhint %}

_Pricing included for your convenience and may be out of date._



| `MAIL_MAILGUN_API_KEY`      | Your Mailgun API key.  |
| --------------------------- | ---------------------- |
| `MAIL_MAILGUN_API_BASE_URL` | Your Mailgun base URL. |

{% hint style="warning" %}
Your base URL should look something like this;

`https://api.mailgun.net/<version>/<domain>/messages`
{% endhint %}
{% endtab %}

{% tab title="SendGrid" %}
[SendGrid](https://sendgrid.com) is a mail provider supported by Feedbacky.

{% hint style="info" %}
Send up to 100 mails per day for _free_ and _forever_.&#x20;
{% endhint %}

_Pricing included for your convenience and may be out of date._



| `MAIL_SENDGRID_API_KEY`      | Your SendGrid API key.  |
| ---------------------------- | ----------------------- |
| `MAIL_SENDGRID_API_BASE_URL` | Your SendGrid base URL. |

{% hint style="warning" %}
Your base URL should look something like this;

`https://api.sendgrid.com/v3/mail/send`
{% endhint %}
{% endtab %}
{% endtabs %}

### Image Compression

| `IMAGE_COMPRESSION_ENABLED` | If you wish to use an Image compression service, `false` by default. |
| --------------------------- | -------------------------------------------------------------------- |
| `IMAGE_COMPRESSION_TYPE`    | At the moment Feedbacky only supports one image compression service. |

{% tabs %}
{% tab title="CheetahO" %}
[CheetahO](https://cheetaho.com) is an image compression service supported by Feedbacky.

{% hint style="info" %}
Compress up to 500 images for _free_ each months.
{% endhint %}

_Pricing included for your convenience and may be out of date._



| `IMAGE_COMPRESSION_CHEETAHO_API_KEY` | Your CheetahO API key. |
| ------------------------------------ | ---------------------- |
{% endtab %}
{% endtabs %}

## Firewall setup

Uncomplicated Firewall (UFW) is the default Firewall on Ubuntu 20.04, it is disabled by default.

1\. Enable Uncomplicated Firewall (UFW).

```
sudo ufw enable
```

2\. Forward the Feedbacky server port.&#x20;

```
sudo ufw allow {SERVER_APP_PORT}/tcp
```

| `{SERVER_APP_PORT}` | Your client application port, if unsure check the [port configuration](installation.md#networking). |
| ------------------- | --------------------------------------------------------------------------------------------------- |

{% hint style="warning" %}
Make sure that you also create a port forwarding rule in your router. If you are using a Virtual Private Server (VPS) check with your provider.
{% endhint %}

## Starting

{% hint style="info" %}
<mark style="color:blue;">**Running in the background**</mark>

tmux
{% endhint %}

1\. Start a new Docker container with Docker Compose.

```
sudo docker-compose up
```

This will take a few minutes depending on your hardware capabilities, your CPU usage will also peak during this process.
