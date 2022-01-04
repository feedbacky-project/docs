---
description: How to run Feedbacky on your own server.
---

# Getting Started

{% hint style="info" %}
You may consider using the development branch of the Feedbacky repository to receive the latest updates, be aware though that these are alpha updates and while used by many in their production servers they could lead to some instability or differ from the final product.
{% endhint %}

## Compatibility

| Operating System | Version          | Status | Notes                                           |
| ---------------- | ---------------- | :----: | ----------------------------------------------- |
| **Ubuntu**       | 20.04            |    ✅   | Installation guide is based on this version.    |
|                  | 18.04            |    ✅   |                                                 |
| **Debian**       | 11               |    ✅   |                                                 |
| **Windows**      | WSL2             |    ✅   |                                                 |
|                  | Server 2022 / 11 |    ✅   | Confirmed working but not officially supported. |
|                  | Server 2019 / 10 |    ✅   | Same as stated above.                           |
| **Mac OS**       | -                |    ❔   | Unconfirmed if working.                         |

## Prerequisites

* Docker [(installation tutorial (just step 1 required))](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
* Docker Compose [(installation tutorial)](https://docs.docker.com/compose/install/)
* MySQL Database [(installation tutorial)](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)
* SMTP mail server **or** account at [Mailgun](https://app.gitbook.com/s/-LumUy4KSgsXd6fywmwU/self-hosting/mailgun.com) **or** account at [SendGrid](https://app.gitbook.com/s/-LumUy4KSgsXd6fywmwU/self-hosting/sendgrid.com)
* An account on at least one of the following services, [Discord](https://discord.com), [GitHub](https://github.com), [Google](https://www.google.com) (for OAuth login since Feedbacky is passwordless)
* (Optional) Swap space for low ram VPS (if your server has less than 2 GB ram, [check tutorial](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-18-04))
* (Optional) Git tool to clone repository from GitHub easily [(installation tutorial)](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-18-04)

## Installation

* Clone our official repository from [Feedbacky repository](https://github.com/Plajer/feedbacky-project) using Git tool. Type `git clone https://github.com/Plajer/feedbacky-project` in the console to download.
* Go to `feedbacky-project` folder you cloned and type `nano .env` to edit configuration file. Once you're in the editor fill `MYSQL_USERNAME` `MYSQL_PASSWORD` and `MYSQL_URL` with proper data.

{% hint style="danger" %}
Please note that `localhost` won't work in `MYSQL_URL` variable due to nature of Docker (container is considered as a remote machine).

IP of server must be provided and MySQL must be configured to accept non localhost connections.
{% endhint %}

{% hint style="info" %}
To allow non localhost connections modify `bind-address` at `/etc/mysql/mysql.conf.d/mysqld.cnf` to `0.0.0.0,`restart mysql service with `service mysql restart` and create MySQL user with `%` login access eg. `'feedbacky'@'%'`.
{% endhint %}

* Fill `JWT_SECRET` with safely generated text at [https://www.grc.com/passwords.htm](https://www.grc.com/passwords.htm) or any other JWT secret token generator page online. **(Random ASCII characters recommended)**
* If you have any other apps running on ports 8090 or 8095 replace `CLIENT_APP_PORT` and `SERVER_APP_PORT` with ports that won't collide.
*   Replace `REACT_APP_SERVER_IP_ADDRESS` with your server IP and `CLIENT_APP_PORT`\
    Correctly set `REACT_APP_SERVER_IP_ADDRESS` should look like:\\

    * :white\_check\_mark:(Correct) http://188.222.333.22:8090
    * :x: (Wrong) http://188.222.333.22:8090\*\*/\*\* (Do not use **`/`** at the end)
    * :x: (Wrong) 188.222.333.22:8090 (**`http://`** is missing)
    * :x: (Wrong) http://188.222.333.22 (No port **`:8090`** provided)

    \
    Check link below if you want to configure Feedbacky to use your own domain name not numeric IP.

{% content-ref url="using-domain-for-feedbacky-instance/" %}
[using-domain-for-feedbacky-instance](using-domain-for-feedbacky-instance/)
{% endcontent-ref %}

* Configure mail server settings, set `MAIL_SENDER` to no reply email you wish to use. Set `MAIL_SERVICE_TYPE` to one of the following types:

| **Type** | **Provider Name**                | **Limits**                       |
| -------- | -------------------------------- | -------------------------------- |
| smtp     | Custom Mail Server               | ------------                     |
| mailgun  | [Mailgun](https://mailgun.com)   | 5k mails/3mo then 0.80$/1k mails |
| sendgrid | [SendGrid](https://sendgrid.com) | 40k mails/1mo then 100 mails/day |

> Pricing last updated 06.2020

* Based on your choice fill proper variables (for Mailgun, `MAIL_MAILGUN_API_KEY` and `MAIL_MAILGUN_API_BASE_URL`, for SMTP `MAIL_SMTP_USERNAME`, `MAIL_SMTP_PASSWORD`, `MAIL_SMTP_HOST` and `MAIL_SMTP_PORT`)
* (Optional) Enable image compression to compress all images sent to the service. Set `IMAGE_COMPRESSION_TYPE` to one of the following types:

| **Type** | **Provider Name**                | **Limits**    |
| -------- | -------------------------------- | ------------- |
| cheetaho | [CheetahO](https://cheetaho.com) | 500 images/mo |

> Pricing last updated 06.2020

* Based on your choice fill proper variables (for CheetahO, `IMAGE_COMPRESSION_CHEETAHO_API_KEY`)
* Click CTRL + x and type `y` to save the file and exit the editor.

### Configuring OAuth apps

Feedbacky is passwordless service and relies on 3rd party services to register and log in. Currently supported OAuth providers are:

* Discord
* Google
* GitHub
* GitLab (since 1.0.0.beta.9)

#### Creating OAuth apps for Feedbacky instance

**Discord**

To enable Discord login support go to [https://discord.com/developers/applications](https://discord.com/developers/applications) and create new application. Create redirect to your Feedbacky instance (IP from `REACT_APP_SERVER_IP_ADDRESS` variable) and include `/auth/discord` at the end.

> [Click here](https://cdn.feedbacky.net/static/mp4/discord-oauth-setup.mp4) for short setup video.

Then to allow logging in via Discord set `OAUTH_DISCORD_ENABLED` to `true` and fill `OAUTH_DISCORD_REDIRECT_URI`, `OAUTH_DISCORD_CLIENT_ID` and `OAUTH_DISCORD_CLIENT_SECRET` with credentials you get from OAuth app.

**GitHub**

To enable GitHub login support go to [https://github.com/settings/developers](https://github.com/settings/developers) and create new OAuth app. Set Authorization callback URL to `REACT_APP_SERVER_IP_ADDRESS` and include `/auth/github` at the end.

> [Click here](https://cdn.feedbacky.net/static/mp4/github-oauth-setup.mp4) for short setup video.

Then to allow logging in via GitHub set `OAUTH_GITHUB_ENABLED` to `true` and fill `OAUTH_GITHUB_REDIRECT_URI`, `OAUTH_GITHUB_CLIENT_ID` and `OAUTH_GITHUB_CLIENT_SECRET` with credentials you get from OAuth app.

**Google**

To enable Google login support go to [https://console.developers.google.com/](https://console.developers.google.com) and create new project. todo

Then to allow logging in via Google set `OAUTH_GOOGLE_ENABLED` to `true` and fill `OAUTH_GOOGLE_REDIRECT_URI`, `OAUTH_GOOGLE_CLIENT_ID` and `OAUTH_GOOGLE_CLIENT_SECRET` with credentials you get from OAuth app.

**GitLab**

To enable GitLab login support go to [https://gitlab.com/-/profile/applications](https://gitlab.com/-/profile/applications) and create new application.

Then to allow logging in via Google set `OAUTH_GITLAB_ENABLED` to `true` and fill `OAUTH_GITLAB_REDIRECT_URI`, `OAUTH_GITLAB_CLIENT_ID` and `OAUTH_GITLAB_CLIENT_SECRET` with credentials you get from OAuth app.

### Running Feedbacky in the background

#### Via screen

You can compile and keep track of Feedbacky instance via `screen` command. Create new screen with `screen -S <screen name>` and go to feedbacky-project folder.

### Compile sources

Begin the source compilation process by doing `docker-compose up`, it will take few minutes depending on your server capabilities.\
Server will likely use high amount of CPU at this part. After successful compilation your Feedbacky instance should be ready.
