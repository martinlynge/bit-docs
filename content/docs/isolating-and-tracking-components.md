---
id: isolating-and-tracking-components
title: Isolating and Tracking Components
permalink: docs/isolating-and-tracking-components.html
layout: docs
category: Getting Started
prev: initializing-bit.html
next: versioning-tracked-components.html
---

Tracking subsets of files as components separates their representation from the repository's file system.

By tracking files that contain an implementation of a component, Bit can isolate a component from the context of the project, and recreate an environment with all the required dependencies for it to function.

This is the first step in versioning and exporting tracked components to be later reused in other projects, without creating additional resources such as repositories and packages.

While component source-code instances can be distributed between many different repositories, components are kept in sync with a single source of truth (we'll get to that later).

## Why isolating components?

- **Isolating** code as a component ensures that they are truly reusable, executable and will work in any other project or context.
- **Versioning** components in your repository creates a higher-level contract for the safe reuse of components in other repositories. For more information on tagging, you can head over to the [versioning tracked components section](/docs/versioning-tracked-components.html) in our docs.
- **Tracking** component changes in a repository means all modifications will be tracked and visible through the [bit status](/docs/cli-status.html) command. This way you can get full visibility for changes made to every component in your project.
- **Understanding components' dependency graph.** By analyzing and understanding the component's dependency graph, Bit can help you understand the exact requirements of a component's packages, files, etc. This also allows Bit to create an isolated environment for each individual component, so you can build, modify and test a component from any context.

## How to track components in a project

Although components with different dependency graphs are all tracked the same way, there are a few differences you should pay attention to. In this section, we will show you how to handle each use-case.

> **Tip**
>
> This section explains how to track components and handle the unique use cases of different dependency graphs using the [bit add](/docs/cli-add.html) command.

## Tracking a simple component

A simple component is a component with no external dependencies. It does not require any external package or file. For example, our `hello-world` component simply contains two files.

```bash
› tree
.
├── package.json
└── src
    ├── hello-world.js
    └── index.js

2 directories, 3 files
```

`index.js`

```js
export {default} from './hello-world';
```

`hello-world.js`

```js
export default function hello(world) {
    return `hello ${world}`;
}
```

In order to track this component, use the [bit add](/docs/cli-add.html) command, list the files that compose this component and set an `ID` for it.

```bash
$ bit add src/hello-world.js src/index.js --id hello/world
tracking component hello/world:
    added src/hello-world.js
    added src/index.js
```

To make sure the component was successfully tracked and isolated, use the [bit status](/docs/cli-status.html) command.

```bash{2}
new components
     > hello/world... ok
```

When Bit specifies that a component status is `ok`, it means that the dependency graph was successfully resolved.

## Tracking multiple components using a single command

Some projects have multiple components in them, and are also structured in that manner. If this is the case in your project (or even for a part of your project), you can apply glob patterns using [bit add](/docs/cli-add.html).

Let's use this folder structure from a random [React](https://reactjs.org) application as an example:

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
        │   ├── Button.js
        │   ├── Button.spec.js
        │   └── index.js
        ├── login
        │   ├── Login.js
        │   ├── Login.spec.js
        │   └── index.js
        └── logo
            ├── Logo.js
            ├── Logo.spec.js
            └── index.js

5 directories, 13 files
```

We can track all these components with a single command.

```bash
$ bit add src/components/* --namespace user
```

Use [bit status](/docs/cli-status.html) to make sure the components' dependency graphs are successfully resolved.

```bash{2,3,4}
new components
     > user/signup... ok
     > user/login... ok
     > user/profile... ok
```

> **Tip**
>
> You can use the [bit add](/docs/cli-add.html) `--namespace` flag to set the components namespace statically or dynamically using the [tracking DSL](/docs/isolating-and-tracking-components.html#tracking-dsl).

## Ignoring files when tracking components

If you are using glob patterns to track components, you might find that some of the files that the pattern finds should not be tracked as part of any of the components. For that you would want to tell Bit to ignore these files. This is done using the `--exclude` flag:

```bash
$ bit add src/component/* --namespace user --exclude *.spec.js
```

> **Files excluded by default**
>
> By default Bit will not track these files and directories: `package.json`, `bit.json`, `node_modules`, `yarn.lock`, `package-lock.json`, `.gitignore`, `.bit.map.json` and `.bitmap`.
> If you are working from within a Git repository, and you keep a `.gitignore` file, Bit will use the paths and filenames listed there, and exclude them by default when tracking components.

## Tracking a component with dependencies

Most components have dependencies. When tracking a set of files as a component, Bit reads through the files and looks for `import` or `require` statements and uses them to figure out whether there are any dependencies for the code. If it finds any, it will figure out if they are either package dependencies or additional files from the project.

Once Bit understands which type of dependency it is, there are several strategies it can act upon. This mainly depends on the type of dependency, and the state of the project workspace.

### Tracking a component with package dependencies

The first added complexity of a component, is if the component is requiring an external package.

An example for a component with a package dependency might look like this:

```bash
.
├── node_modules
|   └── left-pad
├── package.json
└── src
    ├── hello-world.js
    └── index.js
```

`index.js`

```js
export {default} from './hello-world';
```

`hello-world.js`

```js{1}
import leftPad from 'left-pad';

export default function hello(world) {
    return leftPad(`hello ${world}`, 20, '-');
}
```

`package.json`

```js{3}
{
  "dependencies": {
    "left-pad": "^1.2.0"
  }
}
```

In this example:

- The package `left-pad` is located in the project's `node_modules` directory and its version range is set within the project's `package.json` file.
- The file `hello-world.js` requires the package `left-pad`.

Therefore, once you track the files `hello-world.js` and `index.js` as a component, Bit will try to resolve all package dependencies with their versions.

In this example, the version Bit will set for the dependency will be the same version as defined in the project's `package.json` file, meaning the range - `^2.1.0`.

Otherwise, if the package dependency was not defined in the project's `package.json` file, Bit will resolve the version from the package itself from within the node_modules directory and will define the exact version - `2.1.0` (if this is the installed version).

Package dependency resolution **applies automatically** when using [bit add](/docs/cli-add.html) to track components.

```bash
$ bit add src/hello-world.js src/index.js --id hello/world
tracking component hello/world:
    added src/hello-world.js
    added src/index.js
```

To make sure Bit successfully resolved all dependencies, execute the [bit status](/docs/cli-status.html) command.

```bash{2}
new components
     > component/hello-world... ok
```

In order to check what version Bit has resolved for each package dependency, you can use [bit show](/docs/cli-show.html), which will display the component's entire metadata - including its files, dependencies and more.

```bash
┌───────────────────┬─────────────────────────────────────────────────────────────────────┐
│        ID         │                            hello/world                              │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│     Language      │                             javascript                              │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│     Main File     │                      src/hello-world/index.js                       │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│     Packages      │                           left-pad@^2.1.0                           │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│       Files       │       src/hello-world/hello-world.js, src/hello-world/index.js      │
└───────────────────┴─────────────────────────────────────────────────────────────────────┘
```

### Handling missing package dependencies

In some cases, Bit will show the message 'missing package dependency' when running [bit status](/docs/cli-status.html) or [bit tag](/docs/cli-tag.html).

```bash{3,4}
› bit status
new components
     > hello/world... missing dependencies
       missing packages dependencies: left-pad
```

This means that Bit was unable to resolve the component's required package version within the project working directory, and therefore failed to set the version for the newly created component.

As described in the [section above](/docs/isolating-and-tracking-components.html#tracking-a-component-with-package-dependencies), there are a few different strategies that Bit uses for resolving a package version. If it fails to resolve the package version, this error will be shown when using [bit status](/docs/cli-status.html) or [bit tag](/docs/cli-tag.html).

To resolve this issue, please make sure the required package is either defined in the repository's `package.json` file or exists in the `node_modules` directory.

> **Note**
>
> We highly recommend making sure package dependencies are defined in `package.json` to avoid unexpected package version resolution which can potentially cause inconsistencies in your application.

### Tracking a component with file dependencies

A component can depend on other files within a project. These files may have utility functions, global properties and even other components. This means that a tracked component can't be isolated and executable outside of this project's context without these files being tracked alongside it.

Let's use this example to understand why this happens, what Bit will prompt, and how to handle these file dependencies.

```bash
.
├── bit.json
├── package.json
└── src
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        └── noop.js

3 directories, 5 files

```

`index.js`

```js
export {default} from './hello-world';
```

`hello-world.js`

```js{1}
import noop from '../utils/noop';

export default function hello(world) {
    noop();
    return `hello ${world}`;
}
```

`noop.js`

```js
export default () => {};
```

Tracking the `hello-world` component works just the same in this example.

```bash
$ bit add src/hello-world --id hello/world
```

However, once running [bit status](/docs/cli-status.html), you will get a prompt from Bit saying 'untracked file dependencies'.

```bash{3,4}
› bit status
new components
     > hello/world... missing dependencies
       untracked file dependencies: src/utils/noop.js
```

To understand how to handle this matter, let's go through what Bit does in the background when it detects the component's source code `require`/`import` for another file in the project.

1. Bit tries to resolve all imported files of your tracked component.
2. If a required file is already tracked as a part of your component, Bit will not take any action, as the file is already a part of the component.
3. If a required file is tracked by another defined Bit component in your project, Bit will create a component dependency relationship between the newly tracked component and the component containing the required file.
4. If the file is not part of a tracked component, Bit will issue a prompt saying `untracked file dependencies`, as shown above.

Now that you better understand how Bit works behind the scenes, there are two possible courses of action.

### Add an untracked file dependency into a component

If the [bit status](/docs/cli-status.html) command shows you an `untracked file dependency` warning on a component and you've decided that the specific file(s) is part of the component implementation, you can add it to the newly tracked component.

[bit add](/docs/cli-add.html) allows you to append files to be tracked to an existing component. Run the command with the new file, and ensure that you define which component Id you'd like to append the file to.

Let's continue the flow from the [previous section](#tracking-a-component-with-file-dependencies). Adding the `src/utils/noop.js` as a file in the component, append it as follows.

```bash
$ bit add src/utils/noop.js --id hello/world
```

Now you can rerun [status](/docs/cli-status.html), and see that the error is resolved.

```bash
$ new components
     > component/hello-world... ok
```

### Track an untracked file dependency as a component dependency

In some cases, you will see the `untracked file dependency` warning, and realize that the untracked file should be a part of another component that should be tracked by Bit - because it's required by additional components in your project, or maybe just because it has a separate responsibility.

In this case, you can track the file(s) as a new component. The new component will be added as a dependency to the original component.

Due to the fact that the process of detecting component dependencies is automatic ([as explained previously](#tracking-a-component-with-dependencies-in-the-project)), you just need to add a new component and ensure that the untracked file dependency is part of it.

In order to resolve the issue from the previous example, and set `src/utils/noop.js` as a new component dependency for `hello/world` you need to run the [add](/docs/cli-add.js) command.

```bash
$ bit add src/utils/noop.js --id utils/noop
```

The result will be a new component, which is now a dependency for the `hello/world` component.

```bash
› bit status
new components
     > hello/world... ok
     > utils/noop... ok
```

To validate that Bit has added `utils/noop` as a dependency, you can run the following command, and make sure the dependency is defined within the output.

```bash
$ bit show hello/world
```

### Tracking a component with a custom module resolution

If you have configured custom module resolution for your application, for example - in your Webpack config, you will need to define it for Bit as well. This is so Bit will know where to look for the files your code requires.

Let's take our regular example, and update it to use custom module resolution.

```bash
.
├── bit.json
├── package.json
└── src
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        └── noop.js

3 directories, 5 files

```

`index.js`

```js
export {default} from './hello-world';
```

`hello-world.js`

```js{1}
import noop from '@/utils/noop';

export default function hello(world) {
    noop();
    return `hello ${world}`;
}
```

`noop.js`

```js
export default () => {};
```

If you use [bit add](/docs/cli-add.html) to track the `hello-world` component, Bit will prompt an error saying it can't find the dependency `@/utils/noop`. To resolve this, open the `bit.json` file, and add the following configuration:

```js
"resolveModules": {
        "modulesDirectories": ["src"],
        "aliases": {
            "@": "src"
        }
    }
```

Now when you run [bit status](/docs/cli-status.html), you will see that Bit is able to find `utils/noop`, and marks it as an untracked file. So all is left is to track `noop` as [shown here](#track-an-untracked-file-dependency-as-a-component-dependency).

## Tracking a component with test/spec files

Bit promotes and supports matching of test files to a component implementation. Once adding test files, Bit will automatically define all of its dependencies as `devDependencies`, enable [test execution locally and remotely](/docs/testing-components.html) on an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).

To add test files to a component, you can assign them to the component using the `--tests` flag in [bit add](/docs/cli-add.html).

Let's look at this basic example.

```bash{3}
.
├── specs
    └── hello-world.spec.js
└── src
    ├── hello-world.js
    └── index.js
```

```bash
$ bit add src/hello-world src/index.js --tests specs/hello-world.spec.js --id hello/world
```

This way the `hello-world.spec.js` file will be added to the component, but marked as a specification file, so all its dependencies are marked as `devDependencies`, and if you add a test task to the component, Bit will know which file to use.

For more advanced cases, such as tracking multiple components at once, you can use the [Tracking DSL](/docs/isolating-and-tracking-components.html#tracking-dsl), which supports few variables for dynamic definition of namespaces, main file and test files.

## Define a component's main file

Just as with any [CommonJS module](https://nodejs.org/docs/latest/api/modules.html), Bit expects the entry file to be named `index.js` by default for components with more than one file. 
When there's no `index.js` file, we'd need to define a different file as the entry point for our component, using the [bit add](/docs/cli-add.html) `--main` flag.

In the project's structure below, the directory `hello-world` has no `index.js` file.

```bash
.
├── bit.json
├── package.json
└── src
    ├── hello-world
    │   ├── hello-world.js
    └── utils
        └── noop.js

3 directories, 5 files

```

To track the `hello-world` directory as a component with `hello-world.js` as the entry point for the component, use the following command:

```bash
$ bit add src/hello-world --main src/hello-world/hello-world.js --id hello/world
```

This way, Bit will take care of the main file definition without you having to worry about adding extra files to your project or component.

For more advanced cases, such as tracking multiple components at once, you can use the [Tracking DSL](/docs/isolating-and-tracking-components.html#tracking-dsl) - it supports variables for dynamic definition of namespaces, main file and test files.

> **Note**
>
> Bit supports extensions for walking through dependency graphs and file extensions of JS supersets such as TypeScript or CoffeeScript.

## Adding a file to an existing component

Sometimes, you may want to modify a component after already tracking it, and add a new file.
To append a new file to the component, use the [bit add](/docs/cli-add.html) command with the new file and specify the existing component Id using the `--id` flag.

If you'd like to append a new file called `foo.js` to an existing component called `foo/bar`, you can simply use the following command:

```bash
$ bit add src/foo.js --id foo/bar
```

To make sure the new file has been added to your component, you can use [bit show](/docs/cli-show.html) and look for the newly added files under `files`.

```bash{9}
>  bit show
┌───────────────────┬─────────────────────────────────────────────────────────────────────┐
│        ID         │                              foo/bar                                │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│     Language      │                             javascript                              │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│     Main File     │                           src/bar/index.js                          │
├───────────────────┼─────────────────────────────────────────────────────────────────────┤
│       Files       │                        src/index.js, src/foo.js                     │
└───────────────────┴─────────────────────────────────────────────────────────────────────┘

```

## Automatic component ID resolution

When tracking a component with [bit add](/docs/cli-add.html), Bit automatically defines the component Id by using the tracked directory/file name as the component name and the parent directory as the namespace, unless specified otherwise using the `--id` or `--namespace` flags.

To better understand how Bit resolves names, let's assume the following directory structure:

```bash
.
├── bit.json
├── package.json
└── src
    ├── hello-world
    │   ├── hello-world.js
    │   └── index.js
    └── utils
        └── noop.js

3 directories, 5 files

```

If you track the file `noop.js` under the `utils` directory with [bit add](/docs/cli-add.html) without specifying an ID, Bit will automatically resolve the id.

```bash
$ bit add src/utils/noop.js
```

As you can understand from the above, Bit would resolve the ID of the component to `utils/noop` - seeing as `utils` is the parent directory and noop is the added file/directory name.  
To see the defined component Id, you can check out [bit status](/docs/cli-status.html) or [bit list](/docs/cli-list.html) (after the component has been tagged).

```bash
$ bit status
new components
     > utils/noop... ok
```

## Untracking a component

In some cases you might want to untrack a component you've recently added. This can be easily done using [bit untrack](/docs/cli-untrack.html).

```bash
$ bit untrack hello/world
```

> **Note**
>
> Untracking components only works on new components (see [bit status](/docs/cli-status.html) for more information). It will not work for components that you've imported or tagged.
> For removing such components please use [bit remove](/docs/cli-remove.html).

## Tracking DSL

Bit provides a simple tracking DSL for dynamically matching the main file, tests files or even namespaces.

The following variables are available in the DSL.

- **FILE_NAME** represents each of the component's file names. Can be used to match tests and main files to a component by applying each of the component file names on all others.
- **PARENT** represents each of the component's file parent folder names. Can be used to match each file name by applying each of the component file names on all others.

As an example, and in order to understand Tracking DSL better, let's assume the following file system structure.

```bash
>  tree .
.
├── App.js
├── App.test.js
├── components
│   ├── button
│   │   ├── Button.js
│   │   ├── Button.spec.js
│   ├── login
│   │   ├── Login.js
│   │   ├── Login.spec.js
│   └── logo
│       ├── Logo.js
│       ├── Logo.spec.js
├── favicon.ico
└── index.js

4 directories, 13 files
```

As mentioned in the [previous sections](/docs/isolating-and-tracking-components.html#tracking-multiple-components-using-a-single-command), you can use glob patterns to track multiple components at once. In the project structure above, to make things complicated, the main and tests files have different names for each of the components.  
Using the DSL, you can still track these components by using variables, and dynamically set both main and test files.

```bash
$ bit add src/components/* --main src/{PARENT}/{PARENT}.js --tests src/components/{PARENT}/{FILE_NAME}.spec.js --namespace ui
```

Bit will match and dynamically build the path for the main and test files for each component. 

To see all tracked components with their matched main and test files, use [bit status](/docs/cli-status.html) or [bit show](/docs/cli-show.html).

```bash
$ bit status
new components
    > ui/logo... ok
    > ui/login... ok
    > ui/button... ok
```
