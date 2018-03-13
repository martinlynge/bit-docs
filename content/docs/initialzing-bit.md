---
id: initializing-bit
title: Initializing Bit
permalink: docs/initializing-bit.html
layout: docs
category: Getting Started
prev: installation.html
next: isolating-and-tracking-components.html
---
Initializing Bit on a repository adds a virtual layer that tracks and manages components.

To start tracking components in an existing project, go to your project’s root directory and run the [bit init](/docs/cli-init.html) command.

```bash
cd project-root
bit init
```

This will add several resources to your project, including Bit's main configuration file [bit.json](/docs/conf-bit-json.html), Bit's [component store](/docs/initializing-bit.html#component-store) and an internal component mapping file called [.bitmap](/docs/initializing-bit.html#bitmap).

## Component store

Bit's component store is used as an internal store for Bit objects (components, tags, etc).  
The component store is nested within a directory called `.bit`. If you use [Git](git-scm.com), Bit will create its component store under the `.git` directory. This makes Bit work seamlessly with Git, and ensures that Git does not track the contents of the component store.
In case you do not wish bit to nest the component store within the `.git` directory, just use the `standalone` flag when [initializing Bit](/docs/cli-init.html).

```bash
bit init --standalone
```

When initializing Bit with the `standalone` flag, or when working with any SCM other than Git, ensure it ignores the `.bit` directory, so its contents will not be tracked for changes.

## bit.json

[bit.json](/docs/conf-bit-json.html) is Bit's main configuration file. Use it to configure default naming conventions, extensions, etc.

This file should be tracked and managed by your SCM, alongside your project.

> **Tip**
>
> To learn more about the different configurations available in `bit.json`, please see [bit.json](/docs/conf-bit-json.html) documentation page.

## .bitmap

`.bitmap` is a mapping file used by Bit to map the components Bit tracks in your project concrete paths to the files that are a part of the implementation of the component.
Bit uses this file to import and update sourced components in the right paths in your repository, or to preserve locations for components added in the repository for further updates or changes.

Please make sure to track and manage `.bitmap` by your SCM in the root directory of the repository.
