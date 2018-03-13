---
id: conf-bit-json
title: bit.json
permalink: docs/conf-bit-json.html
layout: docs
category: Configuring Bit
---

The `bit.json` file is your [scope](/docs/what-is-bit.html#what-is-a-scope-component-collection)'s configuration file. This document specifies all the different configurations that are set in the file.

## Overview

First of all, let's take a look at the file:

```json
{
    "env": {
        "compiler": "bit.envs/compilers/react@0.0.3",
        "tester": "bit.envs/testers/karma-mocha-react@0.0.18"
    },
    "saveDependenciesAsComponents": "true",
    "packageManager": "yarn",
    "packageManagerArgs": ["--production", "--no-optional"],
    "packageManagerProcessOptions": {
        "shell": true
    },
    "useWorkspaces": "",
    "manageWorkspaces": "true",
    "componentsDefaultDirectory": "components/{namespace}/{name}",
    "dependenciesDirectory": "components/.dependencies",
    "dist": {
        "entry": "src",
        "target": "dist"
    },
    "extensions": {
        "ext-docs-parser": {
            "config": {},
            "options": {
                "core": true
            }
        }
    }
}
```

## Configurations

### env : object

```json
"env": {
        "compiler": "bit.envs/compilers/react@0.0.3",
        "tester": "none"
    },
```

The `env` section configures your [compiler](/docs/ext-compiling.html) and [tester](/docs/ext-testing.html) [environments](/docs/ext-concepts.html#extensions-vs-environments). If you have a compiler/tester configured, it's full component id and version will be listed here. When no tester/compiler is configured, value will be `none`.
Compilers and testers are not configured manually, but rather by [importing](/docs/cli-import.html#import-a-new-environment) them.

### saveDependenciesAsComponents : bool

```json
"saveDependenciesAsComponents": "true"
```

When you [import](/docs/importing-components.html) Bit components, they may depend on other Bit components. 
Those will be installed as npm packages by default, unless you add the `saveDependenciesAsComponents` with a `true` value.

### packageManager : string

```json
"packageManager": "yarn"
```

The `packageManager` section determines which package manager will be used in order to install dependencies.

When not specified, default is `npm`.

### packageManagerArgs : string[]

```json
"packageManagerArgs": ["--production", "--no-optional"]
```

The `packageManagerArgs` section allows you to specify [npm](https://docs.npmjs.com/cli/install) or [yarn](https://yarnpkg.com/en/docs/cli/install) install arguments to be used.

### packageManagerProcessOptions : object

```json
"packageManagerProcessOptions": {
        "shell": true
    }
```

The `packageManagerProcessOptions` section configures additional options for the child-process running the package manager. The available options are the following [execa options](https://www.npmjs.com/package/execa#options): `shell`, `env`, `extendEnv`, `uid`, `gid`, `preferLocal`, `localDir`, `timeout`.

### useWorkspaces : bool

```json
"useWorkspaces": true
```

The `useWorkspaces` section determines whether to use [yarn workspaces](https://yarnpkg.com/blog/2017/08/02/introducing-workspaces/).

### manageWorkspaces : bool

```json
"manageWorkspaces": true
```

The `manageWorkspaces` section determines whether bit lists the component directories in the root `package.json`'s `workspaces` section.
Bit will list there the `componentsDefaultDirectory`, `dependenciesDirectory`, and all [custom import paths](/docs/cli-import.html#import-a-single-component-from-a-remote-scope).
Additionally, bit will mark the root `package.json` as `private: true`.

Relevant only when `packageManager` is `yarn`, and `useWorkspaces` is `true`.

### componentsDefaultDirectory : string

```json
"componentsDefaultDirectory": "components/{namespace}/{name}"
```

The `componentsDefaultDirectory` section configures the directory where Bit will install the components you import.
You can specify a path using our [DSL](), which supports `name` and `namespace`.

### dependenciesDirectory : string

```json
"dependenciesDirectory": "components/.dependencies"
```

The `dependenciesDirectory` configures the directory where Bit will install components that weren't installed directly, but as dependencies to other components.
This is relevant only when [saveDependenciesAsComponents](#savedependenciesascomponents--bool) is set to `true`.

### dist : object

The `dist` section configures:

* `entry` - The entry point for the `dist` files (e.g. if your component has an 'src' directory that contains all the files, you should set is as the entry point).
* `target` - Where the `dist` files will be placed after components are [built](/docs/building-components.html). If not defined, `dist` files will be placed inside the component directory.

### extensions : object

The `extension` section configures your [extensions](/docs/ext-concepts.html). Those will be listed here after your [import](/docs/cli-import.html#import-an-extension) them.
The extension's configuration and options are listed for each extension. You can find out more about these [here](/docs/ext-concepts.html#configuration-and-options).