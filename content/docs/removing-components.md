---
id: removing-components
title: Removing Components
permalink: docs/removing-components.html
layout: docs
category: Getting Started
prev: testing-components.html
next: documenting-components.html
---

There are many cases when its necessary to remove a component. You can remove components either from a local Scope or a remote Scope.

> **Tip**
>
> This section explains how to remove components and handle the unique use cases in different component states using [bit remove](/docs/cli-remove.html) command. To learn more about this command, head over to the [remove section in our CLI reference](/docs/cli-remove.html).

## Removing a component from a remote Scope

In order to remove a component from a [remote Scope](/docs/organizing-components-in-scopes.html#create-a-remote-scope), just specify the [full component id](/docs/isolating-and-tracking-components.html#automatic-component-id-resolution).

```bash
$ bit remove username.your-scope/foo/bar
successfully removed components:
username.your-scope/foo/bar
```

### Handling dependencies

Sometimes other components depend on the removed component.
Let's say username `tom` has two remote Scopes - `utils` and `components`.

#### Removing component with dependencies in the same Scope

For example, `utils` contains component `trim-right`, which depends on component `left-pad`. Both belong to namespace `string`.
Once we'll try removing `left-pad`, Bit will prevent us from doing that, because `trim-right` depends on `left-pad`:

```bash
$ bit remove tom.utils/string/left-pad
error: unable to delete tom.utils/string/left-pad, because the following components depend on it:
tom.utils/string/trim-right
```

If you wish to ignore it and remove anyway, just use the `--force` flag:

```bash
$ bit remove tom.utils/string/left-pad --force
successfully removed components:
tom.utils/string/left-pad
```

#### Removing component with dependencies in other remote Scopes

The Scope `utils` contains component `string/left-pad`, and the Scope `components` contains component `onboarding/login`, which depends on `left-pad`.
Once we'll try removing `left-pad`, we'll see that the component is removed without further ado:

```bash
$ bit remove tom.utils/string/left-pad
successfully removed components:
tom.utils/string/left-pad
```

What's the difference? When the dependent component is in a different Scope, removal will go as planned - That's because a cached version of the removed component will remain on the other Scope, and the dependent component will continue functioning as usual.

> **Note**
>
> You have to be an [owner or a collaborator](/docs/scopes-on-bitsrc.html#permission-types) on the [remote Scope](/docs/organizing-components-in-scopes.html#create-a-remote-scope) in order to be able to remove components from it.

## Removing a component from a local Scope

In order to remove a component from your [local Scope](/docs/what-is-bit.html#what-is-a-scope-collection), just specify the [local component id](/docs/isolating-and-tracking-components.html#automatic-component-id-resolution) (meaning - just namespace and name).

```bash
$ bit remove foo/bar
successfully removed components:
foo/bar
```

### Handling dependencies

What happens when other components in your local Scope depend on the removed components?
Let's say we are trying to remove component `string/left-pad` from our local Scope...

#### Removing a component with a dependent staged component**

Let's say there's another staged component in our local Scope - `string/trim-right`, and it depends on `string/left-pad`. Use `bit remove` to delete it.

```bash
$ bit remove string/left-pad
error: unable to delete string/left-pad, because the following components depend on it:
string/trim-right
```

To bypass it, use the `--force` flag:

```bash
$ bit remove string/left-pad --force
successfully removed components:
string/left-pad
```

#### Removing a component with a dependent component new and exported components

Now, let's say there's a new component (`number/round`) and an exported component (`number/sum`) in the local Scope - both depend on `string/left-pad`.
Let's see what happens:

```bash
$ bit remove string/left-pad
successfully removed components:
string/left-pad
```

Removal went on as planned, and for two different reasons:

* When a new component is dependent on the removed component, Bit just hasn't created the dependencies between the components yet, and has no way of preventing you from removing the component.
* When an exported component is dependent on the removed component, there's already a cached version of the removed component, which will remain after it's removal, for the use of the dependent exported component.

### Removing a staged component from a local Scope

Removing a staged component will remove and untrack it (meaning - it will be removed from the [.bitmap file](/docs/initializing-bit.html#bitmap)). 
If you want Bit to also delete the component files, use the `--delete-files` flag:

```bash
$ bit remove foo/bar --delete-files
successfully removed components:
foo/bar
```

If, on the other hand, you want to keep tracking it as a new component, use the `--track` flag:

```bash
$ bit remove foo/bar --track
successfully removed components:
foo/bar
```

> **Note**
>
> If you've [tracked](/docs/isolating-and-tracking-components.html) and [tagged](/docs/versioning-tracked-components.html) two components, and one depends on the other, removing it will remove its dependency as well.

### Removing a modified component from a local Scope

When you try to remove a modified component from your local Scope, Bit will prevent from doing it, unless the `--force` flag is issued.

```bash
$ bit remove foo/bar
error: unable to remove modified components: foo/bar. please use --force flag.
```

> **Note**
>
> Removing a new component is basically just untracking it, so just use the [untrack command](/docs/cli-untrack.html) for that.