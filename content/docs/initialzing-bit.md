---
id: initializing-bit
title: Initializing Bit
permalink: docs/initializing-bit.html
layout: docs
category: Getting Started
prev: installing-bit.html
next: isolating-and-tracking-components.html
---
Initializing Bit on a repository adds a virtual layer that allows you to track and manage pieces of code as components.

To start tracking components in one of your projects, run [bit init](/docs/cli-init.html) in the project’s root directory.

```bash
cd project-root
bit init
```

Initializing Bit adds several resources to your project, including Bit's main configuration file [bit.json](/docs/conf-bit-json.html), Bit's [local Scope](/docs/initializing-bit.html#bit-local-scope) and an internal component mapping file called [.bitmap](/docs/initializing-bit.html#bitmap).

## Bit local Scope

Bit's local Scope is used as an internal store for Bit objects (components, `bit tags`, etc).  
The local Scope is nested within a directory called `.bit`. If you use [Git](git-scm.com), Bit will create its local Scope under the `.git` directory. This makes Bit work seamlessly with Git and ensures that Git does not track the local Scope's contents.
If you don't want Bit to nest the local Scope within the `.git` directory, use the `--standalone` flag when [initializing Bit](/docs/cli-init.html).

```bash
bit init --standalone
```

> **Note**
>
> When you initialize Bit with the `--standalone` flag or if you are working with an SCM other than Git, ensure your SCM ignores the `.bit` directory, so its contents is not tracked for changes.

## bit.json

[bit.json](/docs/conf-bit-json.html) is Bit's main configuration file. Use it to configure default naming conventions, extensions, etc.

This file should be tracked and managed by your SCM, alongside your project.

Learn more about `bit.json` configurations [here](/docs/conf-bit-json.html).

## .bitmap

`.bitmap` is Bit's locations and paths file, it is used to map the paths of your tracked components and their dependent files.

Bit uses this file to import and update sourced components in the right paths in your repository, and to preserve locations for components added for further updates or changes.

Make sure that you track and manage `.bitmap` with your SCM in the root directory of the repository.
