---
description: Questions and Answers.
---

# FAQs

## Can I host Feedbacky on _X_ panel?

The answer for this question is **no**, it might work but Feedbacky was not designed in mind for that.&#x20;

#### Why?

Web hosting managers such as cPanel or similar won't work since they do not completely support an integrated Docker manager;

{% embed url="https://support.cpanel.net/hc/en-us/articles/360062418794-Can-I-run-docker-on-a-cPanel-server-" %}

And while some plugins might add a full Docker support they won't be official and again, might not guarantee support.

Feedbacky is best intended to be ran on it's own on a VPS or dedicated hardware, it can and will co-exists with other Docker containers.

While using the Linux terminal might be too uncomfortable for some, our guide covers the necessary commands and can be generally followed without any additional efforts.

### What about Pterodactyl?

Even though Pterodactyl utilize Docker with [Wings](https://pterodactyl.io/wings/1.0/installing.html), it does not support two things;

* Docker Compose
* Multi-image eggs (Both [feedbacky-client](https://hub.docker.com/r/plajer/feedbacky-client) & [feedbacky-server](https://hub.docker.com/r/plajer/feedbacky-server) must be ran together)

Without those, Feedbacky can't be self-hosted using Pterodactyl panel.

## How can I restart the Docker container?

Type in your terminal the `docker container stop`

### Screen

If you are using screen and have Feedbacky in the background, connect to that session are end it by doing `CTRL` + `C`, to start Feedbacky again type the following below.

```
docker-compose up
```

