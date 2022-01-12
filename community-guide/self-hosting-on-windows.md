# Self-hosting on Windows

{% hint style="danger" %}
This is a work in progress guide, some crucial information are currently missing!
{% endhint %}

{% hint style="danger" %}
This guide was made by someone who hosted Feedbacky on Windows and is not official in any capacity.&#x20;

Please understand that support will be limited.
{% endhint %}

## Prerequisites

* Docker Desktop [(installation tutorial)](https://docs.docker.com/desktop/windows/install/)
* Docker Compose [(installation tutorial)](https://docs.docker.com/compose/install/)
* MariaDB Database [(installation tutorial)](https://mariadb.com/kb/en/installing-mariadb-msi-packages-on-windows/)
* Git Bash [(download)](https://git-scm.com/downloads)
* SMTP mail server **or** account at [Mailgun](../self-hosting/mailgun.com) **or** account at [SendGrid](../self-hosting/sendgrid.com)
* An account on at least one of the following services, [Discord](https://discord.com), [GitHub](https://github.com), [Google](https://www.google.com) (for OAuth login since Feedbacky is passwordless)
* 200 MB free space for Feedbacky itself
* (Optional) HeidiSQL, a GUI tool to edit and view MySQL tables. [(download)](https://www.heidisql.com)
