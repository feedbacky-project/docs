---
description: Configure your Feedbacky instance to your needs.
---

# Configuring

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

{% content-ref url="../../temporary-archival/domain-setup.md" %}
[domain-setup.md](../../temporary-archival/domain-setup.md)
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

3\. Add a new redirect with the IP address or domain set [here](configuring.md#networking) and include `/auth/discord` at the end.

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

3\. Fill the **Authorization callback URL** with the IP address or domain set [here](configuring.md#networking) and include `/auth/github` at the end.

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

3\. Add a new redirect with the IP address or domain set [here](configuring.md#networking) and include `/auth/gitlab` at the end.

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



This will take a few minutes depending on your hardware capabilities, your CPU usage will also peak during this process.

```
# >>           CLIENT SIDE VARIABLES CONFIGURATION       <<
# Domain or IP address where Feedbacky is hosted, CLIENT_APP_PORT is required here if you don't use port 80 (web port) or domain
REACT_APP_SERVER_IP_ADDRESS=http://example.com
# Name of your self hosted Feedbacky service
REACT_APP_SERVICE_NAME=Feedbacky
# Link to default user avatar image, use %nick% placeholder to replace with User nickname
REACT_APP_DEFAULT_USER_AVATAR=https://static.plajer.xyz/avatar/generator/%nick%

# >>            GENERAL VARIABLES CONFIGURATION          <<
# Port where client application will run, if this port is used replace it with different one
CLIENT_APP_PORT=8090
# Port where server application will run, if this port is used replace it with different one
SERVER_APP_PORT=8095
# Secret token for authentication purposes, you can generate one here https://www.grc.com/passwords.htm
JWT_SECRET=secretPass

# >>         SQL DATABASE VARIABLES CONFIGURATION        <<
# Database credentials
MYSQL_USERNAME=username
MYSQL_PASSWORD=passwd
# Replace <ip_address> <port> and <database_name> with proper values
MYSQL_URL=jdbc:mysql://<ip_address>:<port>/<database_name>?useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=UTF-8

# >>             LOGIN INTEGRATIONS CONFIGURATION        <<
# Feedbacky is passwordless and relies on 3rd party providers to log in, please enable at least 1 of providers below.
# All filled values for each OAuth application will count as an enabled login integration.

# Discord (https://discordapp.com/) login integration enabled
# To create OAuth application go here https://discordapp.com/developers/applications
OAUTH_DISCORD_REDIRECT_URI=
OAUTH_DISCORD_CLIENT_ID=
OAUTH_DISCORD_CLIENT_SECRET=
# Allow users to log in just using email address.
SETTINGS_MAIL_LOGIN_ENABLED=false

# For Google, GitHub, GitLab and other OAuth integrations please refer to https://docs.feedbacky.net and add any variables manually.

# >>       MAIL SENDING FEATURE VARIABLES CONFIGURATION (optional)      <<
# Name of mail sender
MAIL_SENDER="Feedbacky <no-reply@feedbacky.net>"
# Currently available mail services:
# * mailgun - https://www.mailgun.com/ (credit card required to set up account) (5k mails for 3 months, then $0.80 per 1k mails)
# * sendgrid - https://sendgrid.com/ (100 mails per day)
# * smtp - your own SMTP server for sending mails
# * (blank) - for no mail configuration
MAIL_SERVICE_TYPE=
# API key for mail service selected above (not for SMTP)
MAIL_SERVICE_API_KEY=apiKey
# API url for mail service
# For Mailgun: https://api.mailgun.net/v3/<YOUR DOMAIN>/messages (more here https://documentation.mailgun.com/en/latest/api-intro.html#base-url-1)
# For Sendgrid: https://api.sendgrid.com/v3/mail/send
MAIL_SERVICE_API_BASE_URL=baseUrl
# SMTP server credentials
MAIL_SMTP_USERNAME=username
MAIL_SMTP_PASSWORD=passwd
MAIL_SMTP_HOST=host.com
MAIL_SMTP_PORT=587

# >>        IMAGES COMPRESSION VARIABLES CONFIGURATION (optional)       <<
# Is image compression for images enabled? If yes credentials below must be valid.
IMAGE_COMPRESSION_ENABLED=false
# Currently available compressors:
# * cheetaho - https://cheetaho.com/ (free 500 compressions per month)
IMAGE_COMPRESSION_TYPE=cheetaho
# API key from https://cheetaho.com/ service
IMAGE_COMPRESSION_CHEETAHO_API_KEY=apiKey
```

Not yet available for self-hosted instance;

```
# >>        INTEGRATIONS VARIABLES CONFIGURATION
# GitHub x Feedbacky integration, convert ideas to GitHub ideas.
INTEGRATION_GITHUB_CLIENT_ID=
INTEGRATION_GITHUB_CLIENT_SECRET=
INTEGRATION_GITHUB_REDIRECT_URI=
```
