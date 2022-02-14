---
description: Legacy guide for upgrading an older Feedbacky version.
---

# Upgrading to 0.5.0

{% hint style="info" %}
<mark style="color:blue;">**Legacy**</mark>

This section is intended to provide support for those who are upgrading older Feedbacky instances and can be skipped for new installations (1.0.0-RC.0 and up).
{% endhint %}

{% hint style="warning" %}
<mark style="color:orange;">**Upgrading**</mark>

If you are upgrading your instance from 0.1.0 to 0.5.0, **you must** also apply any changes from version [0.2.0](upgrading-to-0.2.0.md).
{% endhint %}

## Notes

### Added

<mark style="color:green;">**+**</mark> A new variable was added and must be present in your `.env` file in order for Feedbacky to work;&#x20;

{% tabs %}
{% tab title="SETTINGS_ALLOW_COMMENTING_CLOSED_IDEAS" %}
**Type**: `boolean`

If enabled, your board users will be able to comment on ideas that are closed.



```
SETTINGS_ALLOW_COMMENTING_CLOSED_IDEAS=false
```
{% endtab %}
{% endtabs %}

### **Changed**

<mark style="color:orange;">**!**</mark> An existing variable in your `.env` file can be edited;

{% tabs %}
{% tab title="REACT_APP_DEFAULT_USER_AVATAR" %}
**Type**: `string`

Sets the avatar of your board users, add the value below if you want to use generated letter avatars based on usernames.&#x20;



```
REACT_APP_DEFAULT_USER_AVATAR=https://static.plajer.xyz/avatar/generator.php?name=%nick%
```
{% endtab %}
{% endtabs %}
