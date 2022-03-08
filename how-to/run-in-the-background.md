---
description: Keeping Feedbacky running in the background.
---

# Run in the background

## Docker

If you don't want to use a terminal multiplexer, you can use a simple Docker command when starting Feedbacky that will keep the container detached.

```
sudo docker-compose up -d
```

| `-d` | The `-d` flag stands for detached, keep in mind that you will need to manually look up your container logs with another command. |
| ---- | -------------------------------------------------------------------------------------------------------------------------------- |

## tmux

### Creating a new session

```
tmux new -s feedbacky
```

{% hint style="info" %}
You can name the session however you'd like, it doesn't need to be `feedbacky`!&#x20;
{% endhint %}

### Detaching from a session

To detach from a session you are currently viewing, press `CTRL` + `B` and than `D`. This keybind will put you back in a normal session.

### Listing existing sessions

```
tmux ls
```

### Attaching to a session

```
tmux attach-session -t feedbacky
```

{% hint style="info" %}
If you are using a different name for your session, make sure to replace `feedbacky`!
{% endhint %}

### Deleting a session

There are 2 ways you can achieve that.

1. By pressing press `CTRL` + `D`.
2. By typing the following;

```
exit
```

## Screen

### Creating a new session

```
screen new -S feedbacky
```

{% hint style="warning" %}
Make sure the `-S` flag is in cap locks!
{% endhint %}

{% hint style="info" %}
You can name the session however you'd like, it doesn't need to be `feedbacky`!&#x20;
{% endhint %}

### Detaching from a session

To detach from a session you are currently viewing, press `CTRL` + `A` and than `D`. This keybind will put you back in a normal session.

### Listing existing sessions

```
screen ls
```

### Attaching to a session

```
tmux -t 10835
```

{% hint style="info" %}
In this case, `10835` is the socket seen using `screen ls`.
{% endhint %}

### Deleting a session

There are 2 ways you can achieve that.

1. By pressing press `CTRL` + `D`.
2. By typing the following;

```
exit
```
