---
description: Questions and Answers.
---

# FAQs

## Can I host Feedbacky on _X_ panel?

The answer for this question is **no**, it might work but Feedbacky was not designed in mind for that.&#x20;

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

## Why can't I further customize my instance?

Before the release of `1.0.0-rc.0` you could customize your Feedbacky instance a little more further.

Feedbacky used to be manually compiled on your VPS or hardware&#x20;

* Customize meta tags (eg. for Discord) Edit [this file](https://github.com/feedbacky-project/app/blob/master/client/public/index.html)
*   Customize email templates Head over [to this folder](https://github.com/feedbacky-project/app/tree/master/server/src/main/resources/mail\_templates) and edit HTML of files as you wish.

    **Be careful not to replace any placeholders like `${host.address}`**
*   Customize/add new OAuth2 providers Check out [this file](https://github.com/feedbacky-project/app/blob/master/server/src/main/resources/oauth\_providers.yml)

    There is no official documentation on this file so please contact developers if you want to add or change anything here.

