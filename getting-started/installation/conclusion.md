---
description: What you learned while following this guide.
---

# Conclusion

## 🎉  Congratulations!

You have completed the Feedbacky self-hosting installation guide! You can start your newly created instance with the command below.

```bash
sudo docker-compose up
```

This will take a few minutes depending on your hardware capabilities, your CPU usage will also peak during this process.

You can reach us with one of the support links if you encountered any issue along the way.&#x20;

{% hint style="info" %}
<mark style="color:blue;">**Running in the background**</mark>

1\. Create a new session. ****&#x20;

```
tmux new -S feedbacky
```

2\. Detach from the session.

`CTRL` + `B` than`D`.

3\. Reattach to the session.

```
tmux attach-session -t feedbacky
```
{% endhint %}

{% hint style="success" %}
Please consider [donating](../../project-overview/donating.md) to Feedbacky!
{% endhint %}

## What you've learned

By following the self-host guide you should also have learned the basics of;

* Installing Docker with Compose, MariaDB and Apache or Nginx, optionally also tmux or Screen.
* Using the `apt` package manager on Ubuntu or any other Debian-based Linux distribution.
* Editing files with the `nano` editor.
* Configuring a MySQL user and table.
* Using the `ufw` package for your firewall.

## What's Next

Read our after installation handbook to get started with your new instance.&#x20;

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
