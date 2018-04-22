---
id: updating-sourced-components
title: Updating Sourced Components
permalink: docs/updating-sourced-components.html
layout: docs
category: Getting Started
prev: importing-components.html
next: merge-changes.html
---

Bit allows you to source components and easily update them, even when you're not the original author. This section explains how it works.

When consuming a Bit component, updating the component code is easy - you can [import the component](/docs/importing-components.html) make local modifications, and then either contribute them back or keep them sourced in your repository - all the while keeping the components synced to their origin for updates.

## Cloning a repository with sourced components

One of the most common scenarios of updating sourced components starts with cloning the repository that contain them. You would, of course, get the components' code exactly as last pushed to the repository, but what you're missing is Bit's inner objects, which are ignored by the VCS.

In order to get the components' updated objects, just use [bit import](/docs/cli-import.html#import-projects-component-objects-from-their-remote-scope) as follows.

```bash
bit import
```

This is similar to `git fetch` - in the sense that it only gets the object, but not the code.

A basic 'update' flow starts with cloning a git repository.

```bash
$ git clone <my repo>
Cloning into 'updates-example'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 14 (delta 0), reused 14 (delta 0), pack-reused 0
Unpacking objects: 100% (14/14), done.
Checking connectivity... done.

$ cd updates-example
$ bit import
successfully imported 2 components
- bit.example/string/left-pad@0.0.1
- bit.example/string/contains@0.0.1
```

## Getting an updated version of a component

If you wish to get the updated version of the sourced components, use [bit import](/docs/cli-import.html#import-projects-component-objects-from-their-remote-scope) to fetch all remote objects of your sourced components:

```bash
$ bit import
successfully imported 2 components
- up to date bit.example/string/is-string
- updated bit.example/string/contains new versions: 1.0.1
```

If you run `bit status`, you will also be notified of the pending update:

```bash
$ bit status
pending updates
(use "bit checkout [version] [component_id]" to merge changes)
(use "bit log [component_id]" to list all available versions)

    > bit.example/string/contains current: 1.0.0 latest: 1.0.1
```

Now issue the `bit checkout` command to use the latest version of `string/contains` in your workspace:

```bash
$ bit checkout 1.0.1 string/contains
successfully switched bit.example/string/contains to version 1.0.4

updated src/pad-left/contains.spec.js
updated src/pad-left/index.js
updated src/pad-left/contains.js
```

### Updating a sourced component that has local changes

In case that you have a sourced component that has been modified locally that has a newer version on the remote Scope, you will need to merge all local changes with the new remote version.  
Merging components is explained in details [here](/docs/merge-changes.html).

## Handling merge conflicts

There are many cases for merge conflicts in component. You can find a complete list of them and how to handle them [here](/docs/sync-components.html).

## Exporting a modified version of a sourced component

When you have a modified component, you [tag](/docs/versioning-tracked-components.html) and [export](/docs/organizing-components-in-scopes.html) it as a new version, to the remote Scope.

### Get remote changes before setting a version

You should check whether there have been any remote changes to the component. To fetch remote changes, use [bit import](/docs/cli-import.html). And [merge](/docs/merge-changes.html) remote changes to the modified component.

> **Check for remote changes **
>
> You can check for updated remote component versions before fetching them to the local Scope using the [bit list --outdated](/docs/cli-list.html/#show-the-local-and-remote-versions-of-all-local-components) flag.

### Tag a new version

In the previous example, you updated `string/contains` sourced component. Now you can modify the component. After you have modified it, set a new version for it, and export back to the remote Scope.

```bash
$ bit tag string/contains --major
1 components tagged | 0 added, 1 changed, 0 auto-tagged
changed components:  bit.example/string/contains@1.1.0

$ bit export bit.example string/contains
exported 1 components to scope bit.example
```

That's it! The sourced component has been modified, and exported back to the remote with a new version.
