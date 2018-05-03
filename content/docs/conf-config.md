---
id: conf-config
title: Configuring Bit
permalink: docs/conf-config.html
layout: docs
category: Configuring Bit
prev: conf-bit-on-the-server.html
next: conf-analytics.html
---

Bit's general configuration affects your communication with the hub.

## Config commands

All configs are sets using the [config](/docs/cli-config.html) command.

## General configuration

* `ssh_key_file` - A path to an ssh key file, defaults to `~/.ssh/id_rsa` (not required).
* `user.email` - The Email of the user, will be saved on the history logs (required).
* `user.name` - The name of the user, will be saved on the history logs (required).
* `user.token` - Authentication token for a [bitsrc.io](bitsrc.io) account. [As shown here](/docs/setup-authentication.html)
* `hub_domain` - The domain of the default hub, defaults to `hub.bitsrc.io` (not required).
* `analytics_reporting` - True/False indication to send anonymous usage data to Bit. [Read More](/docs/conf-analytics.html). By default it is set to `false`.
* `anonymous_reporting` - True/False indication to remove all masks on anonymous usage data, rendering it not-anonymous. [Read more](/docs/conf-analytics.html). By default it is set to `true`.
* `error_reporting` - True/False indication to send anonymous errors data to Bit. [Read More](/docs/conf-analytics.html). By default it is set to `false`.


## Configure your identity

Make sure to set a user name and email address when you [install Bit](/docs/installing-bit.html). Every component's version uses this information, and it’s immutable after you perform `bit tag`.

In order to streamline this process, if you are using Git and have already set up your identity in your global Git configuration (`git config` for `user.name` and `user.email`), By default, Bit will read these settings, and use them.

If you do not use Git, set your user name and email address according to this command:

```bash
$ bit config set user.name "mickey mouse"
$ bit config set user.email mickey@example.com
```
