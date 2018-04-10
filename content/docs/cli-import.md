---
id: cli-import
title: Import
permalink: docs/cli-import.html
layout: docs
category: CLI Reference
prev: cli-export.html
next: cli-install.html
---
Imports a component to your project from a remote Scope.

## Synopsis

```bash
bit import|i [-t|--tester] [-w|--write] [-v|--verbose] [-c|--compiler] [-e|--environment] [--extension] [-p|--path <directory>] [-d|--display-dependencies] [-f|--force] [--dist] [--conf] [--no-package-json] [ids...]
```

## Examples

### Import a single component from a remote Scope

When importing a component, it will be imported to the default location listed in the [bit.json file](/docs/conf-bit-json.html).
Component is imported without its [environments](/docs/ext-concepts.html#extensions-vs-environments) by default.

```bash
bit import username.foo/bar
```

In order to import a component to a specific location, use the `--path` flag:

```bash
bit import username.foo/bar --path src/foo
```

In order to import a component with its environments, use the `--environment` flag:

```bash
bit import username.foo/bar --environment
```

In order to import a component's specific version, use the `@` sign:

```bash
bit import username.foo/bar@1.0.4
```

### Import project's component objects from their remote Scope

The [bit.json file](/docs/conf-bit-json.html) contains a list of the project's sourced components. In order to import all of their updated objects (similar to running `git fetch`) at once.

```bash
bit import
```

#### Update all project's sourced components with the latest version

In order to pull all remote changes for all your project's sourced components, to get their updates into your code, add the `--write` flag to the `bit import` command.

```bash
bit import --write
```

### Import project's environments

The [bit.json](/docs/conf-bit-json.html) file contains the project's [environments](/docs/ext-concepts.html#extensions-vs-environments) ([compiler](/docs/building-components.html) and [tester](/docs/testing-components.html)). In order to import those, don't specify a specific component, and use the `--environment` flag:

```bash
bit import --environment
```

### Import a new environment

In order to import and set a new [environment](/docs/ext-concepts.html#extensions-vs-environments) for your project's components, use the `--tester` and `--compiler` flags:

```bash
$ bit import bit.envs/compilers/babel --compiler
$ bit import bit.envs/testers/mocha --tester
```

### Import an extension

In order to import a new [extension](/docs/ext-concepts.html#what-is-an-extension) for your project, use the `--extension` flag:

```bash
bit import bit.extensions/commands/pack --extension
```

### Pass an extra arguments to npm

In order to pass extra arguments to an npm client, place the arguments after `--`.
  
```bash
bit import -- --production --no-optional
```
## Options

**-t, --tester**

Import a tester [environment](/docs/ext-concepts.html#extensions-vs-environments) component

```bash
bit import bit.envs/testers/mocha --tester
```

**-v, --verbose**

Show a more verbose output when possible.

```bash
bit import username.foo/bar --verbose
```

**-c, --compiler**

Import a compiler [environment](/docs/ext-concepts.html#extensions-vs-environments) component.

```bash
bit import bit.envs/compilers/babel --compiler
```

**-e, --environment**

Install development [environment](/docs/ext-concepts.html#extensions-vs-environments) dependencies (compiler and tester), or import a component AND its environments.

```bash
bit import -e
bit import username.foo/bar --environment
```

**-p, --path <path>**

Import components into a specific directory.

```bash
bit import username.foo/bar --path src/foo
```

**-d, --display-dependencies**

Display the imported dependencies

```bash
bit import username.foo/bar --display-dependencies
```

**-f, --force**

Ignore local changes (import a component's new version even though it was changed locally).

```bash
bit import username.foo/bar --force
```

**--dist**

Write dist files (when exist) to the configured directory

```bash
bit import username.foo/bar --dist
```

**--conf**

Write the configuration file (bit.json).

```bash
bit import username.foo/bar --conf
```

**--no-package-json**

Do not generate package.json for the imported component(s).

```bash
bit import username.foo/bar --no-package-json
```

**--extension**

Import an extension components.

```bash
bit import bit.extensions/commands/pack --extension
```