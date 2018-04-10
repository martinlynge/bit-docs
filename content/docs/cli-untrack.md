---
id: cli-untrack
title: Untrack
permalink: docs/cli-untrack.html
layout: docs
category: CLI Reference
prev: cli-untag.html
---

Untracks a new (not yet tagged) component.

## Synopsis

```bash
bit untrack|u [ids...]
```

## Example

### Untrack a specific newly-added component

```bash
bit untrack foo/bar
```

### Untrack all newly-added components

```bash
bit untrack --all
```