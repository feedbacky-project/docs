---
description: Questions and Answers.
---

# FAQ

## Why did you make Feedbacky?

Announced on December 9th, 2019 and originally named User Voice BETA was the consequence of multiple failed attempts to find a good feedback collecting service for my own Minecraft projects.

It was originally created in pure PHP and due to earlier versions being buggy and a mess, I chose to entirely redo it from scratch with the goals of making it useful for everybody in mind.

I renamed the project to Your Voice, since the former being an already existing company and this time I chose Java as a candidate because it is my best known language. I went with Spring MVC for the backend framework and JSP + Tomcat for the frontend framework.

Ultimately, I re-branded to Feedbacky, which is a much more catchy name and moved to react for the frontend. Feedbacky will always be there to help you collect feedback and issues from your customers.

{% hint style="info" %}
You can look at an archived version of User Voice before it was in early beta stage as a project named [Plajer's Lair User Voice](https://uservoice.plajer.xyz).
{% endhint %}

## Can I host Feedbacky on _X_ panel?

The answer for this question is **no**, it might work but Feedbacky was not designed in mind for that.

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

## Cloud hosted vs Self-hosted

See the difference on our [website](https://feedbacky.net/#pricing).

## Why can't I further customize my instance?

With versions before [`1.0.0-rc.0`](broken-reference/), you had to download the whole Feedbacky repository and manually compile it on your Virtual Private Server (VPS) or dedicated hardware, thus making it really easy to customize elements of Feedbacky out of the box.

All of this changed and we are instead relying on Docker images which are automatically compiled in the project repository thanks to the contribution of [xMikux](https://github.com/feedbacky-project/app/pull/59).

Unless you make a fork, implement your own edits and compile the necessary Docker images yourself, you won't be able to further customize your Feedbacky instance.

## How do I renew my domain?

Certbot will not automatically renew your domain certificate, you can renew it yourself using this command.&#x20;

```
certbot renew
```

{% hint style="warning" %}
<mark style="color:orange;">**Using Nginx?**</mark>

When renewing your certificate make sure to stop the services before proceeding or else you will get an error.

1\. Stop the Nginx process.

```bash
sudo systemctl stop nginx
```

2\. Renew your certificate.

```bash
sudo certbot renew
```

3\. Start Nginx.

```bash
sudo systemctl start nginx
```
{% endhint %}
