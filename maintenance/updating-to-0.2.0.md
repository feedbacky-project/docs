---
description: Legacy guide.
---

# Updating to 0.2.0



{% hint style="warning" %}
This is a legacy guide intended to provide support for people upgrading their Feedbacky instance from before 0.2.0. You can ignore this if doing a fresh installation.
{% endhint %}

## Added

* A new variable was added to the `.env` configuration file and is required to start Feedbacky, add it to your configuration file and save it:

```
MAIL_SENDGRID_API_KEY=apiKey
MAIL_SENDGRID_API_BASE_URL=baseUrl
```

{% hint style="info" %}
If you don't plan to use SendGrid as a mail provider there is no need to modify these values.
{% endhint %}

* Roadmaps were added in this version but Hibernate will auto magically include it in database.
* Migrator will migrate every user automatically to enable mail preferences for a new Mail Notifications feature.

## **Changed**

* When using Mailgun as a mail provider add `/messages` at the end of `MAIL_MAILGUN_API_BASE_URL` variable in your `.env` configuration file, we no longer add this part in code.
