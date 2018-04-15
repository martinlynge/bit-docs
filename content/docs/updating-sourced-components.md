---
id: updating-sourced-components
title: Updating Sourced Components
permalink: docs/updating-sourced-components.html
layout: docs
category: Getting Started
prev: importing-components.html
next: merge-versions.html
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
Merging components is explained in details [here](/docs/merge-versions.html).

## Handling merge conflicts

There are many cases for merge conflicts in component. You can find a complete list of them and how to handle them [here](/docs/merge-versions.html)

## Exporting a modified version of a sourced component

After you've modified a sourced component, you can [tag it](/docs/versioning-tracked-components.html), and then [export](/organizing-components-in-scopes) to a new remote Scope, or to the original one, if you have the appropriate permissions.

In case you're exporting the modified component to its original cope, you should take care of merge conflicts. Currently, Bit doesn't yet know how to handle merge conflicts. This means you should avoid them altogether by not exporting a new version before getting the newest one from the Scope.

> **Checking for updates on a sourced component**
>
> You can easily check whether there have been any changes remotely made to the component using the [bit list](/docs/cli-list.html#show-the-local-and-remote-versions-of-all-local-components) with the `outdated` option.

So, in case there's a newer remote version, the correct order of actions would be:

1. [Get the latest version](#getting-an-updated-version-of-a-component) from the remote Scope.
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
  │bit.example/string/contains        │0.0.1   │0.0.1   │
  ├─────────────────────────────────────────────────┼────────┼────────┤
  │bit.example/string/left-pad        │0.0.1   │0.0.2   │
  └─────────────────────────────────────────────────┴────────┴────────┘
```

We can see `string/left-pad` has a new remote version - 0.0.2. Let's get the new version.

```bash
$ bit import bit.example/string/left-pad
successfully imported one component
- bit.example/string/left-pad@0.0.2
```

After getting the latest version, we can change our code, and then tag and export the new version. Note that exporting a component to a remote Scope requires you to be a collaborator on that Scope.

```bash
$ bit tag string/left-pad
1 components tagged | 0 added, 1 changed, 0 auto-tagged
changed components:  bit.example/string/left-pad@0.0.3

$ bit export bit.example string/left-pad
exported 1 components to scope bit.example
```

That's it! The component had been sourced and modified, and a new version has been exported back to the source-of-truth - the remote Scope.

### Sourcing and modifying someone else's component

The above scenario deals with sourcing and modifying a component and then exporting it back to its original remote Scope. That's only possible if you're a collaborator on that remote Scope. Sometimes, you may want to source and modify someone else's component. This works exactly the same, except for the exporting. You can choose to modify the component and use it locally without exporting, but on the other hand, you can also choose to export it to a different remote Scope, and so create a new source-of-truth.