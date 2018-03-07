---
id: how-concepts-and-terminology
title: Concepts and Terminology
layout: docs
category: How Bit Works
permalink: docs/how-concepts-and-terminology.html
---
This document provides an overview of the source tree layout and the terminology used in Bit.

## Introduction

Bit manages the source code of components between projects organized in many Git repositories. The components are organized in a shared workspace called a `Scope`, where each component has its own isolated environment, containing all its dependencies, specs and documentation. To participate in the process of sharing and syncing code components between Git repositories, each repository should have its own local `Scope`. So all managed components can be isolated, modified and updated.

## Code Component, Scopes and Extensions

### Code Component

The main entity Bit manages. A *code component* is a collection of related files, specification and documentation representing a software functionality. A code component consists of at least a single file, with no dependencies. But it can be as complicated as a set of files with other files they depend on, package dependencies, spec files and documentation.

A code component is a testable and reusable piece of software. It can be almost anything, such as a function, a class, an object, an array, a string, a React or an AngularJS component.

A component has several important parts and properties:

#### Component ID

When tracking a set of files as a component, the component will get its own unique *component ID*.  By default, it will contain the name of the parent directory of the component, and its own implementation file.

When [tracking a component](/docs/cli-add.html), you can set an ID by using the `--id` flag.

```bash
bit add src/component/navigation # will result in component ID component/navigation
bit add src/component/navigation --id ui/nav-bar # will result in component ID ui/nav-bar
```

#### Main File

The component's entry file. For a single-file component, the main file will be the file being tracked. For components with more than one file, Bit requires to know which file is the entry point. By default, Bit will use the programming language's default entry point file name (for example, `index.js` for JavaScript). In case Bit can't determine it by itself, use the `--m` flag when [tracking](/docs/cli-add.html).

```bash
bit add src/foo --main bar.js # adds the contents of the directory foo as a component, and sets the main entry file as bar.js
```

#### Implementation files

The *implementation files* of the component are the core of a Bit component. This is basically the files that are being tracked, and constitute the component itself. Bit reads the implementation files to understand the code's dependencies and structure.

When sharing components between projects, the resources being shared and synced are the files containing the implementation. They are the ones being versioned, imported and exported.

#### Specification

Adding *specification* to a component is crucial to see if a component can work in an isolated environment. On top of that, when importing implementation files to another project and modifying, it's best to keep the specification files of that component alongside it, to see that no breaking changes have been made.

When [tracking a component](/docs/cli-add.html), use the `--t` flag to mark the specification files of the component.

```bash
bit add src/foo/bar.js -t src/foo/bar.specs.js # add component with specs
```

#### Documentation

If you want other developers to understand how to use a component, you should add *documentation*. The most basic form of component documentation is adding a Markdown file, with explanations on how to use the component. Additionally, you can use extensions that can grab JSDocs annotations right from the code, and create documentation based on it.

#### Versioning

See [tags](#tags)

### Local Scope

A *local Scope* is a virtual layer that tracks code components you want to reuse, from your project. Each Scope is capable of resolving the component's dependency graph, and create an isolated environment for it, and may or may not contain a set of configurations and extensions to expand functionality over the components. For example, an extension that can build each individual component (see [extending Bit](/docs/ext-concepts.html)).

Another task of the local Scope is to manage external components that have been imported to the project. If an update is available for the imported component, the local Scope can import it. When a modification for an imported component is required, the local Scope can handle it, and also push the changes back, so that all other local Scopes that are using the component can get updated.

### Remote Scope

Just like a local Scope, a *remote Scope* isolates and manages code components, but instead of being attached to a Git repository, a remote Scope is meant to be hosted on an external host, to be available for all the consumers.

Similar to a remote Git repository, a remote Bit Scope's job is to sync between all the local Scopes that consume code components.

### Tags

A *tag* is Bit's way to manage a component version. The version is set according to the [semantic versioning](https://semver.org/) specs, and the tag is pinned to a component. This helps maintainers and consumers better understand how a new version of a component might affect the project. When a component is being tagged, Bit locks all its dependencies, files and metadata, and packs it all together, so that whenever you import a component, you will get the same dependencies, files, etc. This helps with fighting an ever-growing dependency hell, as all dependency resolution is done when tagging a component, and not when importing it.

### Extensions

*Extensions* are simply code components that can do actions on isolated components in a Scope. They can add additional functionality, such as running the spec files, or building the component into a distributable micro-package.

### Dependencies

*Dependencies* and the dependency graph are a critical part of a component, as code is not truly isolated, but relies on pre-existing infrastructure and code it interacts with.

There are two types of dependencies in Bit.

#### Package Dependencies

*Package dependencies* are external libraries you use in your code. They are usually installed and managed using Package Managers (such as NPM, Ruby Gems, etc). They are a critical part of the infrastructure of a component, as some libraries implement the base APIs that a component needs in order to function. This makes it crucial to log them, in order to properly create an external environment for a component to run.

#### Component Dependencies

A *component dependency* is formed when a component depends on another component. Bit tracks these dependencies directly from the source code of the tracked components. Bit reads all the dependency declarations in the source code, figures out to which file they point, and if that file is contained within another tracked component, a link between both components will be formed.

Read more about it in [here](/docs/how-dependency-management.html).