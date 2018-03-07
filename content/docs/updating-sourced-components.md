---
id: updating-sourced-components
title: Updating Sourced Components
permalink: docs/updating-sourced-components.html
layout: docs
category: Getting Started
prev: importing-components.html
next: building-components.html
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

For example, let's clone our [updates-example](https://github.com/teambit/updates-example) repository. This repository contains two sourced components - `string/left-pad` and `string/contains`. Both can be found on [bitsrc.io](https://bitsrc.io/tomlandau/updates-example).

```bash
$ git clone https://github.com/teambit/updates-example.git
Cloning into 'updates-example'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 14 (delta 0), reused 14 (delta 0), pack-reused 0
Unpacking objects: 100% (14/14), done.
Checking connectivity... done.

$ cd updates-example
$ bit import
successfully imported 2 components
- tomlandau.updates-example/string/left-pad@0.0.1
- tomlandau.updates-example/string/contains@0.0.1
```

## Getting an updated version of a component

If you then wish to get the updated version of the sourced components, use [bit import](/docs/cli-import.html#import-projects-component-objects-from-their-remote-scope) as follows.

```bash
bit import --write
```

Let's try that on our repository.

```bash
$ bit import --write
successfully imported 2 components
- tomlandau.updates-example/string/left-pad@0.0.1
- tomlandau.updates-example/string/contains@0.0.1
```

If you want to only get the updated versions for specific components, just [import a specific component](/docs/cli-import.html#import-a-single-component-from-a-remote-scope).

```bash
bit import username.scope/foo/bar
```

## Exporting a modified version of a sourced component

After you've modified a sourced component, you can [tag it](/docs/versioning-tracked-components.html), and then [export](/organizing-components-in-scopes.html#organizing-components-in-a-single-scope) to a new remote scope, or to the original one, if you have the appropriate permissions.

In case you're exporting the modified component to its original scope, you should take care of merge conflicts. Currently, Bit doesn't yet know how to handle merge conflicts. This means you should avoid them altogether by not exporting a new version before getting the newest one from the scope.

> **Checking for updates on a sourced component**
>
> You can easily check whether there have been any changes remotely made to the component using the [bit list](/docs/cli-list.html#show-the-local-and-remote-versions-of-all-local-components) with the `outdated` option.

So, in case there's a newer remote version, the correct order of actions would be:

1. [Get the latest version](#getting-an-updated-version-of-a-component) from the remote scope.
2. [Tag](/docs/cli-tag.html)
3. [Export](/dpcs/cli-export.html)

For example, let's say someone else has exported a new version for `string/left-pad`. We'll discover that by running `bit list --outdated`.

```bash
$ bit list --outdated
found 2 components in local scope

  ┌─────────────────────────────────────────────────┬────────┬────────┐
  │Id                                               │Local   │Remote  │
  │                                                 │Version │Version │
  ├─────────────────────────────────────────────────┼────────┼────────┤
  │tomlandau.updates-example/string/contains        │0.0.1   │0.0.1   │
  ├─────────────────────────────────────────────────┼────────┼────────┤
  │tomlandau.updates-example/string/left-pad        │0.0.1   │0.0.2   │
  └─────────────────────────────────────────────────┴────────┴────────┘
```

We can see `string/left-pad` has a new remote version - 0.0.2. Let's get the new version.

```bash
$ bit import tomlandau.updates-example/string/left-pad
successfully imported one component
- tomlandau.updates-example/string/left-pad@0.0.2
```

After getting the latest version, we can change our code, and then tag and export the new version. Note that exporting a component to a remote scope requires you to be a collaborator on that scope.

```bash
$ bit tag string/left-pad
1 components tagged | 0 added, 1 changed, 0 auto-tagged
changed components:  tomlandau.updates-example/string/left-pad@0.0.3

$ bit export tomlandau.updates-example string/left-pad
exported 1 components to scope tomlandau.updates-example
```

That's it! The component had been sourced and modified, and a new version has been exported back to the source-of-truth - the remote scope.

### Sourcing and modifying someone else's component

The above scenario deals with sourcing and modifying a component and then exporting it back to its original remote scope. That's only possible if you're a collaborator on that remote scope. Sometimes, you may want to source and modify someone else's component. This works exactly the same, except for the exporting. You can choose to modify the component and use it locally without exporting, but on the other hand, you can also choose to export it to a different remote scope, and so create a new source-of-truth.