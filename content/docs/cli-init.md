---
id: cli-init
title: Init
permalink: docs/cli-init.html
layout: docs
category: CLI Reference
prev: cli-install.html
next: cli-link.html
---
Initializes an empty Bit Scope. This will create a `bit.json` file, and a `.bit` folder, which will contain Bit's objects & models.

## Synopsis

```bash
bit init [-b|--bare] [-s|--shared <group-name>] [-t|--standalone]
```

## Options

**-b, --bare [name]**

Initializes an empty bit bare Scope.

```bash
bit init --bare
```

**-s, --shared <group-name>**

Adds group write permissions to a Scope properly.

```bash
bit init --shared group-name
```

**-t, --standalone**

Creates the [component store](/docs/initializing-bit.html#component-store) outside the `.git` directory.

```bash
bit init --standalone
```