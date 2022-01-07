# Legacy Updates

{% hint style="warning" %}
The following are legacy guides intended to provide support for people upgrading their old Feedbacky instances and can be ignored if doing a fresh installation.
{% endhint %}

## Updating to 0.5.0

{% hint style="info" %}
If you update for example from 0.1.0 to 0.5.0 **you must** apply all changes from [0.2.0](broken-reference) and 0.5.0 not only 0.5.0!
{% endhint %}

### Added

* A new variable was added to the `.env` configuration file and is required to start Feedbacky, add it to your configuration file and save it:

```
SETTINGS_ALLOW_COMMENTING_CLOSED_IDEAS=false
```

{% hint style="info" %}
Replace `false` with `true` if you want to allow users to comment ideas which are closed.
{% endhint %}

### Changed

* The variable `REACT_APP_DEFAULT_USER_AVATAR` present in your `.env` configuration file was changed. If you wan't to use letter avatars please set it to this value:

```
REACT_APP_DEFAULT_USER_AVATAR=https://static.plajer.xyz/avatar/generator.php?name=%nick%
```

## Updating to 0.2.0

### Added

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

### **Changed**

* When using Mailgun as a mail provider add `/messages` at the end of `MAIL_MAILGUN_API_BASE_URL` variable in your `.env` configuration file, we no longer add this part in code.
