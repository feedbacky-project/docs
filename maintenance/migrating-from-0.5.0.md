# Migrating from 0.5.0

{% hint style="warning" %}
This page exists to support people who are upgrading their Feedbacky instance from older versions. You are not concerned if you are doing a fresh installation.
{% endhint %}

## Added

* A new variable was added to the `.env` configuration file and is required to start Feedbacky, add it to your configuration file and save it:

```
SETTINGS_ALLOW_COMMENTING_CLOSED_IDEAS=false
```

{% hint style="info" %}
Replace `false` with `true` if you want to allow users to comment ideas which are closed.
{% endhint %}

## Changed

* The variable `REACT_APP_DEFAULT_USER_AVATAR` present in your `.env` configuration file was changed. If you wan't to use letter avatars please set it to this value:

```
REACT_APP_DEFAULT_USER_AVATAR=https://static.plajer.xyz/avatar/generator.php?name=%nick%
```
