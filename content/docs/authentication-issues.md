---
id: authentication-issues
title: Authentication Issues
permalink: docs/authentication-issues.html
layout: docs
category: Troubleshooting
prev: latest-version
next: install-components
---

There are several things you can do if you encountered `fatal: permission to scope <scopename> was denied` error message.

## Bitsrc.io account issues

Some issues may relate to simply account configuration issues.

### You are not signed up to bitsrc.io

In order to import/export components hosted on [bitsrc.io](bitsrc.io) you need an to have an active account.  
If you haven't signed up already, head over [here](bitsrc.io/signup).

### Wrong username/password combination

In case you are not using [SSH key for authentication](docs/setup-authentication.html), Bit will ask for your username/password combination for your [bitsrc.io](bitsrc.io) account. Make sure you have provided with the correct combination of it.  
In case you have forgotten your password, head to your [setting page](bitsrc.io/settings/profile) to reset it.

### No permission to the Scope

It may be that you do not have permissions to access the scope in question Scope.

- If the Scope is public, you can import component from it, but you have to have write permissions to export to it.
- If the Scope is private, you must have read/write permission in order to import/export components to it.

## SSH keys issues

Bit uses SSH as the communication protocol between Bit and [bitsrc.io](bitsrc.io). In order to make this process works smoothly, you require to either configure Bit to use a specific SSH key, [as seen here](docs/setup-authentication.html).  
There are several configuration issues that may occur if you hit any permission issues when working with SSH keys and remote Scopes.

**If the SSH connection is not established due to issues with SSH keys, Bit will fail to authenticate.**

> *Bit and SSH Agent*
>
> In you are using SSH agent to store and manage your private SSH keys, Bit will communicate with it to use them when opening a remote connection.

### Private SSH key not found

Check for the location of the private SSH key that is either configured to your SSH agent. If the configured path points to a wrong location, Bit will not be able to authenticate.

### SSH Agent process is down

Check if the SSH agent process is running correctly, and you key is configured.  
Run these command to start the process and add the correct private key.

```bash
eval "$(ssh-agent -s)"
ssh-add -K ~/.ssh/id_rsa
```

### SSH key has a passphrase

In case you use `bit config ssh_key_file` to point Bit to the location of your private key, and that key has a passphrase, Bit will not be able to use it properly. In such cases, please refer to using an SSH agent instead.

### No/Wrong public key uploaded to bitsrc.io

Check if you are using the right public SSH key for your profile.