---
id: cli-install
title: Install
permalink: docs/cli-install.html
layout: docs
category: CLI Reference
---
Installs all dependencies for all the sourced components (or for a specific one), whether they were defined in your package.json or in each of the sourced components, and [links](/docs/cli-link.html) them.

## Synopsis

Install all component package dependencies.

```bash
bit install [-v|--verbose] [id]
```

## Examples

### Install dependencies for all the sourced components

```bash
bit install
```

This will install all the dependencies for the sourced components.

### Install dependencies for a specific component

```bash
bit install foo/bar
```

This will install all the dependencies for a specific component - whether they were defined in your package.json or in your [bit.json](/docs/conf-bit-json.html).

## Options

**-v, --versbose**

Shows a more verbose output when possible.

```bash
bit install --verbose
```