---
id: organizing-components-in-scopes
title: Organizing Components in Scopes
permalink: docs/organizing-components-in-scopes.html
layout: docs
category: Getting Started
prev: versioning-tracked-components.html
next: installing-components-using-package-managers.html
---

A [Scope](/docs/what-is-bit.html#what-is-a-scope-collection) is a curated and dynamic collection of shared components. You can think of it as a component “playlist” that works as a remote location for syncing components between your different repositories while keeping one source of truth.

Scopes are easy and cheap to create for any purpose. You can create a Scope for specific projects, ideas, teams or anything you can imagine.  
In order to organize tagged components from your project and make them available for reuse by other people or repositories, you will need to create a remote [Scope](/docs/what-is-bit.html#what-is-a-scope-collection) and export components to it.  
For example, you can create a Scope for components shared from a project your team is working on, for the [UI Components](https://bitsrc.io/bit/movie-app) of your entire organization or even for shared GraphQL schemas.

Remote [Scopes](/docs/what-is-bit.html#what-is-a-scope-collection) can be connected to each other to form a [Scope](/docs/what-is-bit.html#what-is-a-scope-collection) dependency graph. This allows you to create dependencies between components that are hosted on different Scopes. Each Scope caches all the dependencies of its own components to keep all components available and consistent.

## Creating a remote Scope

[Scopes](/docs/what-is-bit.html#what-is-a-scope-collection) are distributed and lightweight and can be [set up on any server](/docs/conf-bit-on-the-server.html).  
You can also freely host your [Scopes](/docs/what-is-bit.html#what-is-a-scope-collection) on the Bit community hub, [bitsrc.io](https://bitsrc.io).

### Self-managed Scope

To set a self-managed Scope, please head over to [Bit on the Server section](/docs/conf-bit-on-the-server.html) in our docs.

### Creating a Scope on bitsrc.io

[bitsrc.io](https://bitsrc.io) is a community hub designed to host and provide greater discoverability for shared components. You can create as many Scopes as you like and manage permissions via the web UI.  
Components hosted on [bitsrc.io](bitsrc.io) can also be installed using the Yarn / NPM client.  
To host a Scope on [bitsrc.io](https://bitsrc.io) simply [signup](https://bitsrc.io/signup) and follow the instructions.

## Organizing components in a single Scope

After versioning components, they will be seen as `staged components` when using [bit status](/docs/cli-status.html) like in the example below.

```bash
bit status

staged components
  > hello/world
  > foo/bar
```

To export all `staged components`, use the [bit export](/docs/cli-export.html) command, and specify the destination.
As an example, to export all staged components to the Scope [bit/movie-app](https://bitsrc.io/bit/movie-app), use the following command.

```bash
bit export bit.movie-app

2 components were exported to Scope bit/movie-app
```

And that's it, all the staged components will be organized on that remote Scope.

## Organizing components in multiple Scopes

In some cases, you may want to export some of your components to different Scopes. You can specify the name of the component, and the remote Scope. By doing so you can be sure that only the component you selected will be exported to the specified target Scope.

```bash
$ bit export bit.movie-app hello/world
component `hello/world` was exported to Scope bit/movie-app
```

## Remove a component from your repository after an export

In some workflows or cases, you may wish to remove a component from your repository's source-code and consume it as a dependency using a package manager, such as NPM or Yarn, after exporting it to a remote Scope.  
To do that, you can use [bit export](/docs/cli-export.html) `--eject` flag like in the following example:

```bash
bit export bit.movie-app --eject

2 components were exported to Scope bit/movie-app
```

After export, the component dependency will be removed from your source-code and written to your project's `package.json`.
