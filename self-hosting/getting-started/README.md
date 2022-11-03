---
description: How to self host your own Feedbacky instance.
---

# Getting Started

{% hint style="success" %}
_**Psst.. Looking for an affordable hosting provider?**_&#x20;

_Get 10% off using code `FEEDBACKY` with_ [_Senior Hosting_](https://billing.senior-host.com/link.php?id=1)_, in exchange you will get affordable and high-performance VPS for Minecraft or Discord bot hosting!_&#x20;
{% endhint %}

Thank you for using Feedbacky! Follow our beginner friendly step by step guide to get started, we have made it in mind for Linux newbies.&#x20;

Consider supporting us by donating or leaving a star on our GitHub repository.

## Compatibility

{% hint style="danger" %}
<mark style="color:red;">**Heads up!**</mark>

Panel users are required to read this [FAQ section](../../project-overview/faq.md#can-i-host-feedbacky-on-x-panel).
{% endhint %}

| Operating System | Version                                       | Status | Notes                                                                                     |
| ---------------- | --------------------------------------------- | :----: | ----------------------------------------------------------------------------------------- |
| **Ubuntu**       | "Focal" 20.04                                 |    ✅   | Installation guide based on Focal.                                                        |
| ​                | "Bionic" 18.04                                |    ✅   | ​                                                                                         |
| **Debian**       | "Bullseye" 11                                 |    ✅   | ​Missing UFW, see "[Additional Software](../dependencies.md#uncomplicated-firewall-ufw)". |
| **Windows**      | WSL2                                          |    ✅   | _Should work, not tested!_                                                                |
| ​                | <p>​Server 2022</p><p><em>Windows 10</em></p> |   🔧   | Working but not officially supported.                                                     |
| ​                | <p>​Server 2019</p><p><em>Windows 10</em></p> |   🔧   | Working but not officially supported.                                                     |
| **Mac OS**       | ​                                             |    ❓   | Unknown and not officially supported.                                                     |

## Prerequisites

* Feedbacky will consume at least `200Mb` of free space.
* Read the [dependencies](../dependencies.md) page, the following must be installed;
  * [Docker](../dependencies.md#docker)
  * [Database](../dependencies.md#mariadb)
  * [Webserver](../dependencies.md#nginx)
* In order to **authenticate**, you will need an account with any of these services;
  * [Discord](https://discord.com)
  * [GitHub](https://github.com)
  * [GitLab](https://about.gitlab.com)
  * [Google](https://www.google.com)
* In order to **use integrations**, you will need an account with any of these services;
  * [GitHub](https://github.com)
* In order to **receive** **notifications**, you will need an account with any of these services;
  * [Mailgun](https://www.mailgun.com)
  * [SendGrid](https://sendgrid.com)

## Downloading

1\. Create a new directory named "feedbacky" in the `/etc` directory and access it.

```bash
sudo mkdir /etc/feedbacky && cd /etc/feedbacky 
```

2\. Fetch the files from our GitHub repository.

<pre class="language-bash"><code class="lang-bash"><strong>sudo curl -O "https://raw.githubusercontent.com/feedbacky-project/app/master/{.env,docker-compose.yml}"</strong></code></pre>

* `.env` is the file containing the Feedbacky variable in which you can configure to your needs.
* `docker-compose.yml` is the instruction file for Docker.
