---
id: quick-start
title: Quick Start
permalink: docs/quick-start.html
layout: docs
category: Getting Started
next: installing-bit.html
---
Learn the basics of working with Bit in your own projects.

To start, choose an existing repository containing components or modules you’d like to share in other projects. We’ll learn how to use Bit to make these components available to use and even develop from other repositories.

You can also use one of our [example projects](/docs/quick-start.html#example-projects), but we recommend starting with your own code.

When done, you will have a collection of the components and modules shared from the project ready to share with your team.

### Install bit

```bash
npm install bit-bin -g
```

See additional [installation methods](/docs/installing-bit.html).

### Init Bit for your project

[Initialize a Bit workspace](/docs/initializing-bit.html) in your project’s directory.

```bash
$ cd project-directory
$ bit init
```

### Track components with Bit

Bit [isolates and tracks](/docs/isolating-and-tracking-components.html) files or sets of files in your repository as components, to later make them available in other projects.
To track components use the `bit add` command. In this example, we’ll use a glob pattern to track all the components in the relevant directory.

```bash
bit add src/components/* # use a glob pattern or a specific path to track multiple components or a single component.
```

Bit automatically scans the files it tracks for their dependencies (packages and files), to resolve their dependency graph and isolate the components.

> **Tip**
>
> Use the [bit status](/docs/cli-status.html) command to verify that each component's dependency graph was successfully built and resolved.

#### Example

Let's track the components `button`, `login` and `logo` in the following project's directory structure.

```bash
$ tree
.
├── App.js
├── App.test.js
├── favicon.ico
├── index.js
└── src
    └── components
        ├── button
        │   ├── Button.js
        │   ├── Button.spec.js
        │   └── index.js
        ├── login
        │   ├── Login.js
        │   ├── Login.spec.js
        │   └── index.js
        └── logo
            ├── Logo.js
            ├── Logo.spec.js
            └── index.js

5 directories, 13 files
```

To track these files as components we can use the [bit add](/docs/cli-add.html) with a glob pattern.

```bash
$ bit add src/components/*
tracking 3 new components
```

### Add build and test tasks

To eliminate the overhead of configuring build and test environments for every component, Bit lets you choose and add build and test tasks which will apply to all the components you share.  

Read more about adding a [build environment](/docs/building-components.html) and a [test environment](/docs/testing-components.html) to your project.  

Here is a list of [pre-made build and test environments](https://bitsrc.io/bit/envs) for you to get started with quickly.  

You can [specify the component's test files](/docs/isolating-and-tracking-components.html#tracking-a-component-with-testspec-files) with the `bit add` command and  `--test` flag, to let Bit know these are test files and should be a part of the component.

### Version components

[Setting a version for tracked components](/docs/versioning-tracked-components.html) locks the state of the components’ files and dependency graph, and sets a semantic version to the components- thus making each component-version immutable. When tagging a component, all of its build and test tasks will run, and only if all is working correctly, Bit will version the components.

```bash
$ bit tag --all 1.0.0
3 components tagged | 3 added, 0 changed, 0 auto-tagged
added components:  components/button@1.0.0, components/login@1.0.0, components/logo@1.0.0
```

### Export versioned components

#### Create a Scope

In order to export your components, you need to create a Scope in which they will be hosted and made available. To create a Scope, create a free account in [bitsrc.io](https://bitsrc.io/signup), and create a Scope.  

A [Scope](/docs/scopes-on-bitsrc.html) is similar to a git repository, that can host and manage components.  

You can learn about [managing components in Scopes](/docs/organizing-components-in-scopes.html) and check out [this example Scope](https://bitsrc.io/bit/movie-app) we created for [React components].

#### Authenticate Bit CLI

In order for the local Bit client to interact with resources you created in your [bitsrc.io](bitsrc.io) account, you need to authenticate the client.  

Use `bit login` to open a login page in the browser. Enter your username and password and return to Bit-CLI to continue.

```bash
$ bit login
Your browser has been opened to visit: http://bitsrc.io/bit-login?redirect_uri=http://localhost:8085...
```

#### Export components

Unlike publishing packages, [exporting](/docs/cli-export.html) a component will *not* change the project's source code or force you to set up any additional repositories.  

Instead, you can use the `bit export` command to share components from you project’s existing structure to [bitsrc.io](bitsrc.io).

```bash
$ bit export username.scopename  # Share components to your Scope
exported 3 components to scope username.scopename
```

### Use components in other projects

#### Install with NPM / Yarn

All components shared with Bit can be installed with the NPM / Yarn client or with any package manager that implements the [CommonJS package registry specification](http://wiki.commonjs.org/wiki/Packages/Registry).

You can now install the components you just exported in other project [using NPM / Yarn](/docs/installing-components-with-package-managers.html). This workflow makes every component and module available to install for every developer, using Bit or not.

#### Import and develop components

Using the `bit import` command you can [import components](/docs/importing-components.html) and modules shared with Bit into any repository, and continue to develop them.  

The sourced and modified component can then be exported back to [bitsrc.io](https://bitsrc.io) as a new version of component, or as a new component.

This workflow makes it easier to maintain and modify components, to build software faster.

#### Update and sync changes

Changes to components made in different projects can be [updated](https://docs.bitsrc.io/docs/updating-sourced-components.html) and synced, and even [merged](https://docs.bitsrc.io/docs/merge-changes.html) between different repositories. This workflow helps your team collaborate and work together.

## Example Projects

Want more examples?

We've created a few example projects that will help you learn how to use bit (don't forget to take a look at each project's `readme` file on [GitHub](github.com)).  

* A [React movie-app](https://github.com/teambit/movie-app) with UI components, and a [matching Scope collection](https://bitsrc.io/bit/movie-app) of components shared from this App with Bit.
* [Ramda with Bit](https://github.com/teambit/ramda), and a [matching collection](https://bitsrc.io/bit/ramda) of individual components from the library.

## Summary

You now know the basics of Bit, and can start putting it to use for sharing code between your projects and with your team.

Feel free to learn more through the different sections of the docs, search the [Bit website](https://bitsrc.io) for components or visit [Bit on GitHub](https://github.com/teambit/bit).
