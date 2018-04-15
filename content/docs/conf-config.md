---
id: conf-config
title: Configuring Bit
permalink: docs/conf-config.html
layout: docs
category: Configuring Bit
prev: conf-bit-on-the-server.html
next: conf-analytics.html
---

Bit's general configuration affects communication with the hub.

## Config command

All configs are set using the [config](/docs/cli-config.html) command.

## General configuration

* `ssh_key_file` - A path to an ssh key file, defaults to `~/.ssh/id_rsa` (not required).
* `user.email` - The Email of the user, will be saved on the history logs (required).
* `user.name` - The name of the user, will be saved on the history logs (required).
* `hub_domain` - The domain of the default hub, defaults to `hub.bitsrc.io` (not required).

## Configure your identity

The first thing you should do when you [install Bit](/docs/installation.html) is set your user name and email address. This is important because every component's version uses this information, and it’s immutable after you perform a tag.

In order to streamline this process, if you are using Git, and already set up your identity in your global Git configuration (`git config` for `user.name` and `user.email`), By default, Bit will use read these settings, and use them.

If you do not use Git, you should set your email and name according to this command:

```bash
bit config set user.name "mickey mouse"
bit config set user.email mickey@example.com
```
