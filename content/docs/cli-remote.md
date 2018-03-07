---
id: cli-remote
title: Remote
permalink: docs/cli-remote.html
layout: docs
category: CLI Reference
---
Displays, adds and removes remote Scopes.

## Synopsis

```bash
bit remote [add <url>] | [rm <name>] [-g|--global]
```

## Examples

### Display existing remote Scopes

```bash
bit remote
```

### Add a remote

Add a remote to the local Scope.

```bash
bit remote add <url>
```

Or add a remote globally

```bash
bit remote add <url> --global
```

Remote name will be the remote Scope name.

## Remove a remote (from local Scope/global config).

```bash
bit remote rm <Scope-name>
```