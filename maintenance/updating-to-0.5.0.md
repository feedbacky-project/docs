---
description: Legacy guide.
---

# Updating to 0.5.0

{% hint style="danger" %}
If you update for example from 0.1.0 to 0.5.0 **you must** apply all changes from [0.2.0](updating-to-0.2.0.md) and 0.5.0 not only 0.5.0!
{% endhint %}

{% hint style="warning" %}
This is a legacy guide intended to provide support for people upgrading their Feedbacky instance from before 0.5.0. You can ignore this if doing a fresh installation.
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
