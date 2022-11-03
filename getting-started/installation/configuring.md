---
description: Configure your Feedbacky instance to your needs.
---

# Configuring

Open your `.env` file and edit the environment variables to fit your needs.

```bash
sudo nano .env
```

## Networking

| `REACT_APP_SERVER_IP_ADDRESS` | The IP address of your Virtual Private Server (VPS) or dedicated hardware or a domain. |
| ----------------------------- | -------------------------------------------------------------------------------------- |
| `CLIENT_APP_PORT`             | Port for the client application, by default it is 8090.                                |
| `SERVER_APP_PORT`             | Port for the server application, by default it is 8095.                                |

### Domain

You can use your own domain with Feedbacky, follow the guide below.

{% content-ref url="../domain-setup.md" %}
[domain-setup.md](../domain-setup.md)
{% endcontent-ref %}

## Security

| `JWT_SECRET` | A random generated token used for authentication purposes. |
| ------------ | ---------------------------------------------------------- |

Use this link to generate one yourself;

{% embed url="https://www.grc.com/passwords.htm" %}
Any other JWT secret token generation can also be used.
{% endembed %}

{% hint style="warning" %}
For extra security, random ASCII characters are recommended. Remember, **do not share** your token with anyone else!
{% endhint %}

## Database

| `MYSQL_USERNAME` | The username set during the database setup process.    |
| ---------------- | ------------------------------------------------------ |
| `MYSQL_PASSWORD` | The password set during the database setup process.    |
| `MYSQL_URL`      | Connection information that will be used by Feedbacky. |

## OAuth

Follow these steps to use the Discord OAuth.

1\. Access Discord's [Developer Portal](https://discord.com/developers/applications).

2\. Create a new OAuth application.

{% embed url="https://cdn.feedbacky.net/static/mp4/discord-oauth-setup.mp4" %}
3\. Add a new redirect with the IP address or domain set [here](configuring.md#networking) and include `/auth/discord` at the end.
{% endembed %}

Follow these steps to use the GitHub OAuth.

1\. Access GitHub's [Developer settings](https://github.com/settings/developers).

2\. Create a new OAuth application.

{% embed url="https://cdn.feedbacky.net/static/mp4/github-oauth-setup.mp4" %}
3\. Fill the **Authorization callback URL** with the IP address or domain set [here](configuring.md#networking) and include `/auth/github` at the end.
{% endembed %}

{% hint style="danger" %}
The Google OAuth guide is not yet available.
{% endhint %}

Follow these steps to use the GitLab OAuth.

1\. Access GitLab's [Developer Portal](https://gitlab.com/-/profile/applications).

2\. Create a new OAuth application.

3\. Add a new redirect with the IP address or domain set [here](configuring.md#networking) and include `/auth/gitlab` at the end.

4\. Fill the necessary variables with your newly created Discord OAuth application.

| `OAUTH_GITLAB_ENABLED`      | Disabled by default, to use GitLab OAuth change to `true`.    |
| --------------------------- | ------------------------------------------------------------- |
| `OAUTH_GITLAB_REDIRECT_URI` | Your instance domain with `/auth/gitlab` included at the end. |
| `OAUTH_GITLAB_CLIENT_ID`    | Your OAuth client ID.                                         |
| `OAUTH_GITLAB_SECRET`       | Your OAuth client secret.                                     |

## Mail

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
Send up to 100 mails per day for _free_ and _forever_.
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

## Image Compression

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

## Integrations

{% hint style="info" %}
Integrations are only available for cloud-instance users at the moment!
{% endhint %}
