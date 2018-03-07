---
id: cli-test
title: Test
permalink: docs/cli-test.html
layout: docs
category: CLI Reference
---
Runs the tests of the specified component(s) using the configured tester.

## Synopsis

```bash
bit test|t [-v|--verbose] [id]
```

## Examples

### Run tests on all your project's components

```bash
bit test
```

### Run tests on a specific component

```bash
bit test foo/bar
```

## Options

**-v, --verbose**

Shows npm verbose output for inspection.

```bash
bit test foo/bar --verbose
```