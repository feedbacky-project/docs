---
description: Creating a new user and database for Feedbacky.
---

# Database Setup

1\. Start the SQL shell.

```bash
sudo mysql
```

2\. Create a new user for Feedbacky.

{% hint style="info" %}
As stated [previously](../dependencies.md#additional-configuration), we won't be able to create a `localhost` user, instead we'll use the `%` wildcard instead.
{% endhint %}

```sql
CREATE USER '{MYSQL_USERNAME}'@'%' IDENTIFIED BY '{MYSQL_PASSWORD}';
```

| `{MYSQL_USERNAME}` | The username of your choice. |
| ------------------ | ---------------------------- |
| `{MYSQL_PASSWORD}` | The password of your choice. |

3\. Create a new database for Feedbacky.

```sql
CREATE DATABASE feedbacky;
```

4\. Grant all privileges to your MySQL user.

```sql
GRANT ALL PRIVILEGES ON feedbacky.* TO '{MYSQL_USERNAME}'@'%';
```

5\. Flush the privileges.

```sql
FLUSH PRIVLEGES;
```

{% hint style="info" %}
Flush will reload the privileges without having the need to restart the MariaDB process.
{% endhint %}

6\. Exit the SQL shell.

```
exit
```
