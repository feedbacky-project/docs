---
description: Legacy guide for upgrading an older Feedbacky version.
---

# Upgrading to 0.2.0

{% hint style="info" %}
<mark style="color:blue;">**Legacy**</mark>

This section is intended to provide support for those who are upgrading older Feedbacky instances and can be skipped for new installations (1.0.0-RC.0 and up).
{% endhint %}

## Notes

### Added

<mark style="color:green;">**+**</mark> A new variable was added and must be present in your `.env` file in order for Feedbacky to work;&#x20;

{% hint style="info" %}
If you don't plan to use SendGrid as a mail provider there is no need to modify these values, just add them to your configuration file.
{% endhint %}

{% tabs %}
{% tab title="MAIL_SENDGRID_API_KEY" %}
**Type**: `string`

Replace the `apiKey` value with your actual key.



```
MAIL_SENDGRID_API_KEY=apiKey
```
{% endtab %}

{% tab title="MAIL_SENDGRID_API_BASE_URL" %}
**Type**: `string`

Replace the `baseUrl` value with the appropriate URL.



```
MAIL_SENDGRID_API_BASE_URL=baseUrl
```
{% endtab %}
{% endtabs %}

<mark style="color:green;">**+**</mark> Roadmaps were added in this version but Hibernate will auto magically include it in database.

<mark style="color:green;">**+**</mark> Migrator will migrate every user automatically to enable mail preferences for a new Mail Notifications feature.

### **Changed**

<mark style="color:orange;">**!**</mark> If you use Mailgun as your mail provider, you must edit an existing variable in your `.env` file;

{% tabs %}
{% tab title="MAIL_MAILGUN_API_BASE_URL" %}
**Type**: `string`

Add `/messages` at the end of the end of your Mailgun base URL, as this portion is no longer handled by code.



```
MAIL_MAILGUN_API_BASE_URL=https://api.mailgun.net/<version>/<domain>/messages
```
{% endtab %}
{% endtabs %}
