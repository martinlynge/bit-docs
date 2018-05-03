---
id: versioning-tracked-components
title: Versioning Components
permalink: docs/versioning-tracked-components.html
layout: docs
category: Getting Started
prev: isolating-and-tracking-components.html
next: organizing-components-in-scopes.html
---

Versioning tracked components in a repository helps create a higher-level contracts, for the safe reuse of components between different repositories and users.

Versioning components is essential when collaborating on components to avoid breaking the different applications the components are sourced to or used in. Component versioning can get complex in complex dependency graphs and Bit does a lot to make this process as easy and efficient as possible.

Once a Bit component is tracked, you can version it using the [bit tag](/docs/cli-tag.html) command.  
Upon tagging, Bit will apply the actions below to make sure that the component dependency graph is resolved. It will also validate that the component is isolated and can be reused outside of the repository's context.

## What happens when a component is tagged

- **Version is set for the component.** Bit uses [SemVer](https://semver.org) to version components and provides a simple CLI interface to control SemVer for multiple components at once.
- **Resolve and lock the component's dependency graph.** Bit will make sure all component dependencies are resolved. All dependencies will be written and locked for better predictability and consistency.
- **Component execution and testing in an Isolated component environment.** To make sure each component is truly reusable and executable outside of the original project's context, Bit will build an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment) and apply all configured extensions. Extensions may include testing execution, compiling or even docs parsing for better discoverability.

> **Bit and SemVer**
>
> Bit uses [SemVer](https://semver.org) to tag components versions and implement strategies to control them. See [bit tag](/docs/cli-tag.html) to learn more about various versioning options.

## Versioning a simple tracked component

A simple component is a component with no external dependencies, as shown in [this example](/docs/isolating-and-tracking-components.html#tracking-a-simple-component).
Once a component is successfully tracked, and the [bit status](/docs/cli-status.html) command shows the component's status as new or modified, then the component is ready to be versioned.

```bash
$ bit status
new components
    > hello/world
```

To version our new `hello/world` component, use [bit tag](/docs/cli-tag.html).

```bash
$ bit tag hello/world 1.0.0
1 components tagged | 1 added, 0 changed, 0 auto-tagged
added components:  hello/world@1.0.0
```

As you can see, this command tagged the new component `hello/world` in version `1.0.0`. 
By using [bit status](/docs/cli-status.html), you can now see the component is staged and pending to be organized in a Scope.

```bash
$ bit status
staged components
    > hello/world
```

> **Note**
>
> In case a specific version was not specified, Bit will increment a patch version by default.
> This means that in the above case the command `bit tag hello/world` will tag the component in version `0.0.1`.
> To increment major or minor versions you can use the `--minor` or `--major` flags as described on the [Bumping component versions](/docs/versioning-tracked-components.html#bumping-component-versions) section, below.

## Versioning multiple components

In many cases, we'll want to make cross-component changes or [track multiple new components](/docs/isolating-and-tracking-components.html#tracking-multiple-components-using-a-single-command) together.
To ease the process of versioning multiple new or modified components, [bit tag](/docs/cli-tag.html) exposes the `--all` flag.

`--all` flag indicates to Bit to tag all new and modified components together. It can accept a specific SemVer for all components or increment each of the component's versions individually.

To understand this better, let's take the following component status as an example:

```bash
$ bit status
new components
    > hello/world
    > ui/button
modified components
    > string/pad-left
```

In this case, we have two new components (`hello/world` and `ui/button`) and one modified component (`string/pad-left`). To version these components by running [bit tag](/docs/cli-tag.html) take a look at the following example:

```bash
$ bit tag --all
3 components tagged | 2 added, 1 changed, 0 auto-tagged
added components:  hello/world@0.0.1, ui/button@0.0.1
changed components: string/pad-left@1.0.2
```

As you can see, since a specific version was not specified, Bit increased all new and modified components in a single patch version.

> **Note**
>
> As [bit tag](/docs/cli-tag.html) applies only to new and modified components, it can't be used for aligning versions of all the components under the same Scope.
> To tag and align all versions under the same Scope, head over to the [align local components](/docs/versioning-tracked-components.html#aligning-all-local-components-to-the-same-version) section below.

## Versioning components in a complex dependency graph

Components that depend on other components, such as [this example](/docs/isolating-and-tracking-components.html#tracking-a-component-with-file-dependencies) are called 'components with dependency tree'. If you have this structure in your components, you might sometime see that a component you haven't modified yourself is marked as modified. That's because a dependency of that component has been changed, and thus the dependency tree has been modified.

For example, when component `foo` requires component `bar`, and both are in the same project, and the component `bar` is modified - `foo` will be marked as modified as well, when we run the [bit status](/docs/cli-status.html) command. However, the component's source code hasn't changed at all, but rather its dependency graph has.

This is a very basic example, but the same principal applies when there are multiple levels of dependencies between components. Bit will understand how a modification to any component on the dependency graph of the project will affect the rest of the components used, and prompt to [tag](/docs/cli-tag.html) the changes.

For example, let's assume the following project structure and components.

```bash
.
├── bit.json
├── package.json
└── src
    └── utils
        ├── bar.js
        └── foo.js
```

utils/bar.js

```js
console.log('hello');
```

utils/foo.js

```js
import Bar from './bar';
```

To set up the working state for the components, we need to [track](/docs/cli-add.html) and [tag](/docs/cli-tag.html) both components:

```bash
bit add src/utils/*.js --namespace utils
bit tag --all 0.0.1
```

We now have two components with a dependency between them.

Let's modify `utils/bar.js`, as follows:

```js
console.log('hello world');
```

Now, when running [bit status](/docs/cli-status.html), you will see that both components are modified, and will be required to [tag](/docs/cli-tag.html) both of them.

## Aligning all local components to the same version

After a while of working with Bit, each component will have a life of its own, and its own list of tags. However, with every major release, you might want to tag all components with the same version, so your consumers will know that this component is a part of a specific release.

As `--all` only applies on modified or new components, you may also want to include non-modified or new components in the versioning process.

To do this, [bit tag](/docs/cli-tag.html) exposes the `--scope` flag that enables the tagging of all components under your project's Scope.

A simple example for using this flag would be:

```bash
bit tag --scope 1.0.1
```

In this example, all the Scope's components will be tagged with version 1.0.1.

For better understanding, let's assume we have the [status](/docs/cli-status.html) output as shown on [versioning multiple components section](/docs/versioning-tracked-components.html#versioning-multiple-components) above:

```bash
$ bit status
new components
    > hello/world
    > ui/button
modified components
    > string/pad-left
```

We have with two more components (`user/login` and `user/signup`) already tagged, without applied changes.

You can view all your local components by using [bit list](/docs/cli-list.html).

```bash
bit tag --scope 3.0.0
```

## Bumping component versions

The great thing about Semantic Versions is that the version number itself tells your consumers the type of change to expect. Usually, setting it is not an issue, as you know the version of the project you are working on, so you know which number to increment.

When working with small pieces of your project, you might not always remember which exact version each component has. Bit allows you to simply state the type of change you made, and it will bump the version accordingly.

Bumping versions is done with [bit tag](/docs/cli-tag.html) `--minor`, `--patch` and `--major` flags. For more concrete examples, please take a look at the below section.

### Examples

```bash
bit tag --all --major          # Increment all modified and new components with a major version.
bit tag --all --minor          # Increment all modified and new components with a minor version.
bit tag --all --scope --patch  # Increment all components with a patch version.
bit tag --scope --patch        # Increment all components under the same Scope with a patch version.
```

## Unversioning components

If you regret tagging a version, you can easily undo it, as long as the component is still staged (meaning - there's a tagged version that hasn't been exported yet). This is called 'untagging', and is done using the [untag command](/docs/cli-untag.html).

> **Note**
>
> Untagging a version won't undo the code changes associated with that version - it will only remove the version itself.

### Untagging a component's specific version

Let's say we've just tagged version 1.0.0 for component `foo/bar`:

```bash
bit tag foo/bar 1.0.0
```

If we want to untag this version, the latest one, we can just [untag](/docs/cli-untag.html) the component and specify which version we want to untag.

```bash
bit untag foo/bar 1.0.0
```

This will revert the component's 1.0.0 version. You can untag a version whether it's the latest one or not, as long as it's still staged.

### Untagging a component's staged versions

In case we want to untag all the component's staged versions - whether there are 10, or just the one - we can untag the component without specifying a version.

Let's say we'd just [imported](/docs/importing-components.html) component `foo/bar` while its latest version was 1.0.0. Afterwards, we'd made a few sets of changes and have tagged the component three minor versions.

```bash
bit tag foo/bar
bit tag foo/bar
bit tag foo/bar
```

The component's version will now be 1.0.3, since we've tagged version 1.0.1, 1.0.2 and 1.0.3.
Now let's untag all of them.

```bash
bit untag foo/bar
```

This will remove those three staged versions, and revert the component's version to the latest one that's not staged - 1.0.0.

> **Component status changed after untagging**
>
> Once you've untagged all the component's staged versions, the code changed will remain. Therefore, the component's status will revert back to `new` or `modified`.

### Untagging a specific version for all the staged components

We can untag a specific *staged* version from all the staged components at once, by specifying the `--all` option and a specific version.

```bash
bit untag --all 0.11.4
```

This is especially useful if we've [aligned all local components to the same version](#aligning-all-local-components-to-the-same-version) and wish to revert it. 

### Untagging all the staged versions for all the components in your local scope

If we want to revert ALL the versions we've tagged across the whole local scope, we can use the `-all` option, without specifying a version.

```bash
bit untag --all
``` 

## Viewing component history

Bit provides simple tooling to view component tag history.

```bash
bit log hello/world
```
