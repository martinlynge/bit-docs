---
id: cli-export
title: Export
permalink: docs/cli-export.html
layout: docs
category: CLI Reference
prev: cli-diff.html
next: cli-import.html
---
Pushes staged component(s) to a remote Scope.

## Synopsis

```bash
bit export [-f|--forget] [-e|--eject] <remote> [id...]
```

## Examples

### Export all staged components to the same Scope

```bash
bit export [Scope name]
```

### Export a specific component to a Scope

```bash
bit export [Scope name] [component id]
```

### Export two (or more components) to a Scope

```bash
bit export [Scope name] [component id 1] [component id 2]
```

### Eject component back as a dependency

In some workflows or cases, you may wish to remove a component from your repository's source-code and consume it as a dependency using common package managers such as NPM or Yarn after exporting it to a remote Scope. In order to do that, use the `--eject` flag.

```bash
bit export bit.examples string/pad-left --eject
```

## Options

**-f, --forget**

Don't update `bit.json` after export.

```bash
bit export username.Scope-name foo/bar --forget
```

**-e, --eject**

Remove the component from the repository and consume it as a dependency using a common package manager.

```bash
bit export bit.examples string/pad-left --eject
```
