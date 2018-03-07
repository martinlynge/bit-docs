---
id: quick-start
title: Quick Start
permalink: docs/quick-start.html
layout: docs
category: Getting Started
next: installation.html
# next: what-is-bit.html
---
Learn how to use Bit in a few simple steps.

These simple steps will teach you the basics you need to know for working with Bit, all the way from sharing components right from your project to using them in any other project with your favorite tools.
When finished, you will also have a beautiful gallery of components ready to share with your team.

Let's get started with these 4 quick steps:

## Install bit

```bash
npm install bit-bin -g
```

See additional [installation methods](/docs/installation.html).

## Choose components to share

### Init Bit for your project

Run this command to [initialize a Bit workspace](/docs/initializing-bit.html).

```bash
cd project-directory
bit init
```

### Track files with Bit

Bit [tracks and isolates](/docs/isolating-and-tracking-components.html) files in your repository and set them as components.  
To track components use the `bit add` command.

```bash
bit add src/components/* # use a glob pattern or a specific path to track multiple components or a single component.
```

Bit automatically scans the files it tracks for their dependencies (packages and files), to figure out their dependency graph.

> **Tip**
>
> Use the `bit status` [command](/docs/cli-status.html) to make sure that each component's dependency graph was successfully built and resolved by Bit.

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

To track these files as components we'll use the `bit add` [command](/docs/cli-add.html) with a glob pattern.

```bash
$ bit add src/components/*
tracking 3 new components
```

### Add build and test tasks

Bit supports managing build and test tasks for each component. You can add a Bit build/test task to your project, and they will be set and run for each tracked component.

Read more about adding [build environment](docs/building-components.html) and [test environment](docs/testing-components.html) to your project.

Here is a list of [curated build and test environments](https://bitsrc.io/bit/envs), for you to get started quickly.

You can [specify the relevant test files](/docs.bitsrc.io/docs/isolating-and-tracking-components.html#tracking-a-component-with-testspec-files) for your components in the `bit add` command to let Bit know they should be part of your components.

### Version a component

[Setting a version for a component](/docs/versioning-tracked-components.html) locks the state of the component files and the dependency graph, and sets a semantic version to the component, thus making each component-version immutable. When tagging a component, all its build and test tasks will run, and only if all is working correctly, Bit will version the component.

```bash
$ bit tag --all 1.0.0
3 components tagged | 3 added, 0 changed, 0 auto-tagged
added components:  components/button@1.0.0, components/login@1.0.0, components/logo@1.0.0
```

## Export versioned components

### Create a Scope

In order to export a component, you need to [create a free account in bitsrc.io](https://bitsrc.io/signup), and afterward create a Scope. A Scope is a remote Bit workspace that can host and manage components.

There is more to learn about [managing components in Scopes](/docs/organizing-components-in-scopes.html). You can check out some example Scopes for **[React components](https://bitsrc.io/bit/movie-app)**.

### Export components

Unlike publishing packages, this will not change the project's source code or force you to set up any additional repositories.

```bash
$ bit export username.scopename  # Share components to this Scope
exported 3 components to scope username.scopename
```

## Use components in other projects

### Install with Yarn / NPM

Components shared to Bit can automatically be installed with the NPM / Yarn client or with any package manager that implements the [CommonJS package registry specification](http://wiki.commonjs.org/wiki/Packages/Registry).

You can now install the components you just exported them [with NPM / Yarn](/docs/installing-components-with-package-managers.html). This allows projects and developers which are not familiar with Bit, to still use Bit components.

It is also possible to [source the components directly to another project](/docs/importing-components.html), using Bit. If you choose to source your components, you can also modify them later from the consumer project, and export the modifications back to [bitsrc.io](bitsrc.io) as a new version for that component.

## Example projects

Want more examples? We've created a few example projects that will help you learn how to use bit (don't forget to take a look at each project's `readme` file on [GitHub](github.com)):
* A [React movie-app](https://github.com/teambit/movie-app) with UI components, and a [matching collection](https://bitsrc.io/bit/movie-app) of components shared from this App with Bit.
* [Ramda with Bit](https://github.com/teambit/ramda), and a [matching collection](https://bitsrc.io/bit/ramda) of individual components from the library.

## Summary

You are now ready to work with Bit and easily share code between your projects.  
Feel free to learn more through the different sections of the docs, search the [Bit website](https://bitsrc.io) for components or visit [Bit on GitHub](https://github.com/teambit/bit). 