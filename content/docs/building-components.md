---
id: building-components
title: Building Components
permalink: docs/building-components.html
layout: docs
category: Getting Started
prev: merge-versions.html
next: testing-components.html
---

Bit components can be built from any execution environment using Bit's [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).

The build process of bit components is done in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment), using [compilers](/docs/ext-compiling.html), environments - a special kind of [extension](/docs/ext-concepts.html#extensions-vs-environments).

In order to build a component, you need to have a compiler defined.

### Defining a default compiler for your project

A compiler is defined per [Scope](/docs/scopes-on-bitsrc.html). Once defined, building a component within that Scope will build the component using the defined compiler.
In order to do that, just [import the compiler](/docs/cli-import.html#import-a-new-environment):

```bash
$ bit import bit.envs/compilers/babel --compiler
the following component environments were installed
- bit.envs/compilers/babel@0.0.7
```

You can now go take a look at the `env` configuration in your [bit.json](/docs/conf-bit-json.html#env--object) and see your compiler over there.

#### Curated list of build environments on Bitsrc.io

You can find a list of Build Environments maintained by the [bitsrc.io](bitsrc.io) team [here](https://bitsrc.io/bit/envs).

- [Babel](https://bitsrc.io/bit/envs/compilers/babel).
- [Flow](https://bitsrc.io/bit/envs/compilers/flow).
- [Preact](https://bitsrc.io/bit/envs/compilers/preact).
- [React](https://bitsrc.io/bit/envs/compilers/react).
- [React TypeScript](https://bitsrc.io/bit/envs/compilers/react-typescript).

### Building your component

Once you have a defined compiler, you can [build your component](/docs/cli-build.html):

```bash
$ bit build foo/bar
foo/bar
dist/foo/bar.js
dist/foo/bar.js.map
```

## Building a component remotely

When staging or exporting components, Bit runs When staging or exporting components, Bit runs the components' extensions in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment). So the `build` process for the component will run outside of the context of the project. This will help to understand how separated the component truly is.

### Self-managed Scope

After exporting a component to a [self-managed remote Scope](/docs/organizing-components-in-scopes.html#self-managed-scope), the component will be built in the remote Scope in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).

### bitsrc.io-managed Scope

After exporting a component to a [bitsrc.io-managed Scope](/docs/organizing-components-in-scopes.html#creating-a-scope-on-bitsrcio), [bitsrc.io](bitsrc.io) will create an isolated container, where the component will be built in the remote Scope in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).
As the Scope [owner or collaborator](/docs/scopes-on-bitsrc.html#Scope-permissions), you can enter the component page on [bitsrc.io](bitsrc.io), and view the build log in the `Console Output` tab.

## Set dist target and entry

You can set your `dist` target and entry by editing the [bit.json](/docs/conf-bit-json.html).

## Implementing a component compiler

You can implement your own custom compiler. [See here](/docs/ext-compiling.html) for more details.