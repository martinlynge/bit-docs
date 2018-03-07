---
id: conf-bit-on-the-server
title: Bit on the Server
layout: docs
category: How Bit Works
permalink: docs/conf-bit-on-the-server.html
---

Bit is up and foremost a collaboration tool. In order to actually collaborate bit components, you'll need to set up a remote server (well, to be technically correct - you can import/export components directory from individual Scopes, but it's hard to manage and use).

## Collaboration using Scopes

The best collaboration method is to set up a remote Scope, where all the consumers will have access to import and export components from. In this section, we'll refer to the 'remote Scope' as 'bit server'. However, you will notice that the amount of resources used by a remote Scope is so small, that you rarely need an entire server for it.

To set up a bit server you will need to decide on the type of protocol you need. Currently bit supports 2 protocols, which you will need to choose between, according to your workflow. Then, use this simple guide to run the commands required to create and connect the bit server.

Due to the fact that a bit server in used only as a collaboration tool, a bit server is basically a bare repository. Meaning - a bit Scope with no working directory.

## Protocols

Bit currently supports 2 types of communication protocols: Local and SSH.

Each protocol has its own use cases, and here you'll learn when to use each.

### Local Protocol

Bit's local protocol is based on accessing folders and files in your local file system. This means that a bit server which uses the local protocol will not be accessible for users that want to connect to the bit server from outside of the machine. So using this protocol is best for local work only, such as evaluating bit or developing bit components.

To use it, simply connect to the bit server by using [this command](/docs/cli-remote.html#add-a-remote):

```bash
bit remote add file://<path>/<to>/<remote>/<Scope>
```

> *For Windows computers*
>
> `bit remote add file://C:\scopes\my-remote-scope`

### SSH Protocol

A common transport protocol for Bit when self-hosting is over SSH. This is because SSH access to servers is already set up in most places – and if it isn’t, it won't take long to do so. SSH is also an authenticated network protocol; and because it’s ubiquitous, it’s generally easy to set up and use.

This makes SSH the preferred protocol for collaboration when developing components to remote Scopes.

To add a bit server over SSH, run this command:

```bash
bit remote add ssh://bit-username@bit-server:/<path>/<to>/<remote>/<Scope>
```

## Setting up a Bit server

There are two main options to work with Bit on the server. The first one is to use [bitsrc.io](https://bitsrc.io) to host and manage your Scopes and components, or you can set up Bit on your own server.

### Hosting code components on Bitsrc.io

[Bitsrc.io](https://bitsrc.io) is a community hub dedicated to hosting and managing code components using Bit. You can create a free account, and host any amount of code components, with managed build/test task runners.

1. Head over to [bitsrc.io](https://bitsrc.io) and open a free account.
2. Create a free Scope.
3. Follow the instructions to export a component to your Scope.

You will not need to do any special setup on your local Bit Scope, as Bit comes with a default resolver that looks for Scopes in [bitsrc.io](https://bitsrc.io).

### Setting up a server

In this section, we'll explain how to use a remote server to setup as many Scopes as needed.

#### Prerequisites

1. *nix server.
2. User for bit (with .ssh and authorized_keys, for remote workflow).

#### Create a remote Scope

Once you have all prerequisites, the one thing left is to run the [bit init](/docs/cli-init.html) command with the `--bare` flag, to create a Scope without a working directory.

```bash
su bit
mkdir /opt/bit
cd /opt/bit
mkdir first-scope
cd first-scope
bit init --bare
```

Now add your own public SSH key to the authorized_keys list of the user bit. This will allow you to import and export components hosted in first-scope.

Let's add our first-scope as a remote Scope to a newly create local Scope on our development machine. We'll assume that our server is named bit-server, for the sake of this example.
Don't forget to [specify the bit username](https://github.com/teambit/bit/issues/682).

```bash
mkdir my-project
bit init
bit remote add ssh://bit-username@bit-server:/opt/bit/first-scope
```

Now, once you have your remote set up for Bit, you can [export components](/docs/cli-export.html) there.

```bash
bit export first-scope
```