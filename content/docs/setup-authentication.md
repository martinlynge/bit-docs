---
id: setup-authentication
title: Setup Bitsrc Authentication
permalink: docs/setup-authentication.html
layout: docs
prev: bitsrc-component-ci.html
next: my-account.html
---

[Bit](https://github.com/teambit/bit) is open source and distributed, Scopes can be set up on any machine.

You are welcome to host your Scopes and components on [bitsrc.io](https://bitsrc.io), the free community hub for Bit.
[bitsrc.io](https://bitsrc.io) provides many features, including Scope permissions, component search engine, auto-parsed docs and examples, test results and live rendering for React components.

Before working with [bitsrc.io](https://bitsrc.io) you will first need to configure your Bit client and SSH connectivity.

## Configure your local Bit client

To set your username and email in Bit, use the [bit config command](/docs/cli-config.html).

> **Using Git config**
>
> If you already have a configuration for `user.name` and `user.email` in your `git config`, Bit will automatically use the predefined properties in Git.

```bash
$ bit config set user.name "mickey mouse"
$ bit config set user.email mickey@example.com
```

## Set up bitsrc.io connectivity

### Signup to bitsrc.io

[Create a free account](https://bitsrc.io/signup) on [bitsrc.io](https://bitsrc.io).

## Connect Bit CLI to your Bit account

Bit uses SSH to authenticate a computer with your account, so Bit can authentication with the Scopes hosted on [bitsrc.io](https://bitsrc.io).

SSH access to servers is already set up in most cases – and if it isn’t, it’s easy to do. SSH is also an authenticated network protocol; and because it’s ubiquitous, it’s generally easy to set up and use.
This makes SSH the preferred method for collaboration.

There are two methods to authenticate a local Bit client to your [bitsrc.io](bitsrc.io) account.

- [bit login](/docs/cli-login.html) command - This generates an authentication token for your [bitsrc.io](bitsrc.io) account, configures it to the local Bit client and uses that token to initiate an SSH connection.
- SSH key-pair - Generate an SSH key pair, upload the public part to your [bitsrc.io](bitsrc.io) account, and configure your Bit client to use the private part.

### Authenticating Bit using 'bit login'

To authenticate your Bit client, run the following command:

```bash
$ bit login
Your browser has been opened to visit: http://bitsrc.io/bit-login?redirect_uri=http://localhost:8085...
```

Your browser will automatically open a login page. Enter your [bitsrc.io](bitsrc.io) account credentials. The authentication token will be generated and configured to [bit config](/docs/cli-config.html).

> **Logging out from Bit**
>
> To quickly logout from Bit, run the [bit logout](/docs/cli-logout.html) command.

#### Managing your authentication devices

If you used [bit login](/docs/cli-login.html) to authenticate a Bit client with a [Bit](bitsrc.io) account, you can see a list of all logged in devices in the [profile settings](https://bitsrc.io/settings).

Here you can remove tokens, thus forcing Bit clients to re-authenticate themselves with your account.

### Authenticating Bit using SSH key-pair

If you know how to generate your SSH key, you can skip the next part and move directly to [authenticate your SSH Key to bitsrc.io](#upload-public-ssh-key-to-bitsrcio).

#### Generate SSH key for macOS/Linux

To generate an SSH key follow these steps:

1. Open a terminal application.
2. Run this command (replace ‘email’ with the email associated with your BitSrc account):

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

3. Accept the default location for the key file.
4. To add a private key to the SSH-agent please follow the steps below:

Start the SSH agent: `eval "$(ssh-agent -s)"`

Add the private key we’ve created in the last step: `ssh-add ~/.ssh/id_rsa`

#### Generate SSH key for Windows

To generate an SSH key, please follow these steps:

1. Download and start the [puttygen.exe generator](https://winscp.net/eng/docs/ui_puttygen).
2. In the "Parameters" section choose **SSH2 DSA** and press **Generate**.
3. Move your mouse on the small screen in order to generate the key pairs.
4. Enter a key comment, which will identify the key (useful when you use several SSH keys).
5. Click "Save private key" to save your private key.
6. Click "Save public key" to save your public key.

#### Upload public SSH key to bitsrc.io

1. First, you should log in to your [bitsrc.io](https://bitsrc.io/login) account.
2. Click on your user icon to open your user actions menu:

<img src="https://storage.googleapis.com/bit-docs/SSH%20connect%201.png" width="212" height="220" margin=20 />

3. Click on the ‘Settings’ link to reach your user settings section. Once inside, click on ‘SSH Keys’ in the left pane:

<img src="https://storage.googleapis.com/bit-docs/ssh%20key%202.png" width="483" height="271" margin=20 />

4. In the ‘SSH Keys’ section, you will notice that there are no SSH keys associated with your account. Click on ‘new SSH key’:

<img src="https://storage.googleapis.com/bit-docs/ssh%20key%203.png" width="525" height="142" margin=20 />

5. In the ‘new SSH key’ form, type in a name for the key. 
The key name documents the key, and will not affect the behavior of the system.

<img src="https://storage.googleapis.com/bit-docs/ssh%20key%204.png" width="483" height="337" margin=20 />

6. To Fill the ‘Key’ form, go back to where you created your SSH key pair on your machine, and copy the content of the file named id_rds.pub. 
Paste it in the form. 

7. Click on ‘Add SSH key’ to submit. 
You will now see a new item in the SSH key list:

<img src="https://storage.googleapis.com/bit-docs/ssh%20key%205.png" width="585" height="301" margin=20 />

You are now connected via SSH and can export and import components from the [bitsrc.io](https://bitsrc.io) community hub.