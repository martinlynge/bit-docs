---
id: importing-components
title: Importing Components
permalink: docs/importing-components.html
layout: docs
category: Getting Started
prev: installing-components-using-package-managers.html
next: updating-sourced-components.html
---

Importing a component to a repository enables you to source any component, in any repository while keeping all changes synced.

Bit makes component development easy and accessible from any repository. It provides a distributed component development workflow, which means each component can be sourced and developed from any repository while still preserving one source of truth, which is located in the component's remote Scope.  
For example, you can make local modifications to components and then either contribute them back or keep them sourced in your repository. All the while keeping the components synced to their origin for updates.  
You can imagine this workflow as if you were able to "inject" any dependency and source it right into your repository with a single command, and "eject" it back as a dependency after making your changes.

To understand this better, let's import the component [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad) and source it in the following project's directory structure.

```bash{5}
$ tree .
.
├── bit.json
├── package.json
└── src
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        ├── noop.js
        └── pad-left.js

3 directories, 6 files
```

To import the component [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad) into the `src` directory, use [bit import](/docs/cli-import.html) like in the example below.

```bash
bit import bit.examples/string/pad-left --path src/pad-left
```

[bit import](/docs/cli-import.html) sources the component to the given path. In this case, the component [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad) will be sourced to `src/pad-left`.

```bash{6,7,8,9,10}
$ tree .
.
├── bit.json
├── package.json
└── src
    ├── pad-left
    │    ├── is-string.js
    │    ├── pad-left.js
    │    ├── pad-left.spec.js
    │    └── index.js
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        └── noop.js

3 directories, 6 files
```

As you can now see, the component [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad) is now sourced in this repository, ready for changes to be made.  
To require this component you can use either relative paths or absolute paths (see [Linking components section](/docs/importing-components.html#linking-components) below).

> **Note**
>
> Imported components should be tracked by your SCM (Git, Mercurial, etc.).
> Updates would still be available through merge once applicable.

## Importing different component versions

When importing a component, its latest version will be imported by default. If, however, you want to import an older version, you can easily do that by specifying the version after an `@` sign:

```bash
bit import bit.examples/string/pad-left@0.0.11
```

## Dependencies

Components can have dependencies. Bit automates the installation of component dependencies using package managers (Yarn or NPM).

```bash
bit install
```

[bit install](/docs/cli-install.html) installs all dependencies, whether they were defined in your package.json or in each of the sourced components.  
By default, Bit would try to use Yarn to leverage [Yarn Workspaces](https://yarnpkg.com/lang/en/docs/workspaces/) for optimized installation of all dependencies. Otherwise, if an installation of Yarn was not found, Bit would use NPM.

> **Note**
>
> To learn more on how to configure NPM and Yarn to install Bit components, please head over to [Install Components using Package Manager section](/docs/installing-components-using-package-managers.html) on our docs.

### Yarn workspaces

[Yarn Workspaces](https://yarnpkg.com/en/docs/workspaces) is a feature in Yarn that allows to install dependencies from multiple `package.json` files in subfolders of a single root `package.json` file.  
As components have separate `package.json` files, Yarn Workspaces is ideal for the installation of component dependencies.  
To configure Bit to use Yarn for the installation of dependencies, configure the following in your project's [bit.json](/docs/conf-bit-json.html#packagemanager--string).

```js{2}
{
  "packageManager": "yarn"
}
```

### NPM

Dependencies can be also configured to be installed with NPM.  
To configure Bit to use NPM for the installation of dependencies, configure the following in your project's [bit.json](/docs/conf-bit-json.html#packagemanager--string) file.

```js{2}
{
  "packageManager": "npm"
}
```

## Bindings

In reality, although our code may be built for modularity, files may require each other relatively like in the example component [string/left-pad](https://bitsrc.io/bit/utils/string/left-pad/code).

```js
import isString from '../is-string';
```

Relative paths couple a component to a specific project directory structure and therefore makes the component hard to reuse from other project structures.  
Bit aims to make code sharing frictionless by isolating components in their original context.  
To do this, whenever relative import statements are being used in components, Bit generates a special kind of files called `bindings` to make sure the component dependencies would be resolved from any project structure without delivering redundant or duplicated code.

For example, the [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad) component above depends on the component [string/is-string](https://bitsrc.io/bit/utils/validation/is-string) and uses a relative import statement to call it.  
To make sure [string/is-string](https://bitsrc.io/bit/utils/validation/is-string) is resolved correctly, Bit creates a binding file in the correct path of the component that includes an absolute import statement to the dependency.

`is-string.js`

```js
module.exports = require('@bit/bit.examples.string.is-string');
```

To reduce this binding file, simply use the absolute path above in the component's source code instead of the relative path and tag a new version for the component.

> **Tip**
>
> To reduce and flatten component file system structure, you can modify your code to use absolute paths.
> For more information, check out [Linking components section](/docs/importing-components.html#linking-components) below.

## Linking components

Once a component is imported to a repository, Bit would generate a link in your `node_modules` directory by the package name - pointing to the component path in your project's directory structure.  
Links are generated by default when importing components or by explicitly running [bit link](/docs/cli-link.html) like in the example below.

```bash
bit link
```

Linking components in `node_modules` allows you to import a component using an absolute path and avoid changing import paths once "ejecting" the component to be a dependency of your project and vice-versa.

For example, after importing the component [string/pad-left](https://bitsrc.io/bit/utils/string/left-pad), a link would be generated and you would be able to import the component from any file in your project like in the following example.

```js
import padLeft from '@bit/bit.examples.string.pad-left';
```

If absolute paths were used, after exporting the component to be used as an external dependency, everything would work properly and the dependency would be resolved correctly by the node module system.

Another aspect of linking, also performed by [bit link](/docs/cli-link.html), deals with links between components and their sourced dependencies.

For example, let's say we have a component, `bit.examples/string/pad-left`, that depends on another component - `bit.examples/string/curry`. We want to update the `curry` component, so we'll source it:

```bash
bit import bit.examples/string/curry
```

This allows us to update the code, but what happens if we want to check its dependent's behavior? `pad-left` consumes `curry` from the `node_modules` directory, which means it will invoke the old code.
Once we run [bit link](/docs/cli-link.html), `pad-left` will consume `curry` from the updated sourced component.

## Default directory convention

You can configure a default directory for component sourcing in your project's [bit.json](/docs/conf-bit-json.html) configuration file.

```js{2}
{
  "defaultComponentPath": "src/{namespace}-{name}"
}
```

Once applying the configuration above, any new imported component will be placed according to this defined convention.  
Let's import the example component `string/pad-left` without specifying the target path.

```bash
bit import bit.examples/string/pad-left
```

This component would be sourced in your project's structure according to this convention

```bash{6,7,8,9,10}
$ tree .
.
├── bit.json
├── package.json
└── src
    ├── string-pad-left
    │    ├── is-string.js
    │    ├── pad-left.js
    │    ├── pad-left.spec.js
    │    └── index.js
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        └── noop.js

3 directories, 6 files
```

As you can see in the example above, the component was sourced according to the defined convention and sourced in directory `string/pad-left`.

## Tracking component changes

Once a component is imported, changes can be made to the component while Bit would continue to track them.  
For example, if after importing `string/pad-left`, any of its file gets modified, run `bit status`, to see that Bit register the component as `modified`.

```bash
$ bit status
modified components
  > string/pad-left
```

## Versioning sourced components

If a component is listed as modified, and the modification needs to be exported back to the remote Scope, you will need to `tag` the component and `export` it to the Scope. Just as you'd do to a newly tracked component.

```bash
bit tag string/left-pad
bit export bit.utils
```

### Fetching updated objects for sourced components

When you start working with a project that has sourced Bit components, and the project itself is managed by an SCM tool, you will need to fetch the remote object of each component, in order to avoid merge conflicts when you modify and export new versions for them (similar to `git fetch`).  
In order to do that, you will need to run the `import` command, without any parameters, as follows.

```bash
bit import
```

> *Overwrite component source code*
>
> If you want to overwrite local sourced components when running `bit import`, simply add the `--write` flag. This will tell Bit to update all local sourced components with their new versions.

## Eject component back as a dependency

With Bit, it's possible to eject a component to being used as an external dependency after an [export](/docs/cli-export.html).

```bash
bit export string/pad-left bit.examples --eject
```

To learn more on exporting and ejecting components, please head over to [this section](/docs/organizing-components-in-scopes.html#remove-a-component-from-your-repository-after-an-export) in the docs.

## Comparing component changes

When you have a modified component, and you are not sure why it is listed as 'modified', you can use the `bit show <component id> --compare` flag. This will show you the difference in the component's meta-data, and help you understand why the component is listed as modified.

The component may be modified due to changes within the files it is tracking, or updates to its dependencies (packages or other components).

> **Using `git diff`**
>
> If you have a sourced component within your project, you can also use the normal `git diff` command, and see any changes that made to the files the component tracks.