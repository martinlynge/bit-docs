---
id: about-bit
title: About Bit
permalink: docs/about bit.html
layout: docs
category: What is Biut
prev: documenting-components.html
    ## next: installation.html
---

We believe that code should be written once and evolve over time. This means that we need to share code between projects and repositories.

We usually share code by creating packages. Creating packages is a complex process that involves multiple repositories, and publishing each one of them separately. This process makes it harder to modify and change shared code.

**Bit works with Git and NPM to make it easy to share code and sync changes between projects at any scale**.

Instead of creating new repositories for your packages, Bit lets you isolate and share any part of any repository (by defining it as a [component](/docs/what-is-bit.html#what-is-a-code-component)), and use your favorite package manager to install it in other projects.

Bit gives you the tools to change code you share, from *any* repository in your codebase and easily sync the changes across your projects.

With Bit, managed code sharing becomes as simple as copy-pasting.

*Bit is a [collaborative open source project](https://github.com/teambit/bit), actively maintained by a venture-backed team and used by different organizations and OSS communities*.

## How Does It Work?

### Faster code sharing

Bit enables you to isolate and share components of code (subsets of files) straight from your project repository, instead of splitting your project into multiple repositories just to publish packages.

These are the 2 key features that makes it possible:

- **Dependency definition** - Bit [automatically resolves](/docs.bitsrc.io/docs/isolating-and-tracking-components.html) and defines the dependency tree for the components you isolate, including both package dependencies and other files from your project.

- **Isolated environment** - Bit creates an [isolated environment](/docs.bitsrc.io/docs/ext-concepts.html) for the code you share. This allows you to continue developing the components from any project while maintaining their own isolated dependency graph. 
For example, components written in typescript can be sourced and developed in a project written in flow-typed.

### Installing components in other projects

Once Bit isolates code components from your project, you can share them to a remote source of truth called a [Scope](#what-is-a-scope-component-collection). You can set up a Scope as local workspace, or use Bit’s [hosting Hub](https://bitsrc.io). From there, your components can be [installed](/docs.bitsrc.io/docs/installing-components-using-package-managers.html) using NPM or Yarn in any of your projects.

### Modifying the code from any repository

Bit decouples the representation of the code you share from your project file structure.
As a result, you can modify the code you share from any repository in your codebase.

Use `bit import` to bring the component's source code into any repository, modify it and share it back to the remote Scope to sync the changes across your projects. You can think of it as “automated and managed copy-pasting” that creates a distributed development workflow.

### Discoverability and control

Code you share with Bit is hosted and organized in your remote Scopes, these can be made available to your entire team. Bit also provides improved discoverability through a search engine and useful visual information for your shared code, including auto-parsed docs and examples, test and build results and even live rendering for UI components, see ([here](https://bitsrc.io/bit/movie-app)).

Since Bit tracks the code you share throughout your codebase, you can easily learn which components are used by who and where, and make vast changes to multiple components together with universal control over your dependency graph.

### Extending Bit

You can extend Bit to integrate with your favorite dev tools to build, test, bundle, lint, pack, publish and optimize the workflow around shared code any way you choose.

## Getting Started

[QUICK START GUIDE](/docs.bitsrc.io/docs/quick-start.html)

## About Components & Scope

### What is a code Component?

A code component is a small, reusable and encapsulated piece of code. Components should handle a single responsibility, representing a well-defined functionality and composable with other components to build larger things.

Every day, more and more of the applications we build are designed with encapsulated, reusable components. From UI and Web components to Node.js modules, utility functions, GraphQL APIs and even serverless functions, these components are our building blocks.

The JS community has done significant progress towards component-driven architectures. From the [nodeJS module system](https://nodejs.org/api/modules.html) to native [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) and  great frameworks like [React](https://reactjs.com), [Angular](https://angular.io) and [Vue](https://vuejs.org/).

Yet, sharing components adds significant overhead and is therefore many times avoided. In Bit, we're trying to make the process of sharing components as easy as copy-pasting while preserving discoverability and maintainability at scale.

Guidelines for building components were wonderfully described by Addy Osmani in his post about the [FIRST principle](https://addyosmani.com/first/).

More information on component design can be found at the [external resources] (/community/external-resources.html) section.

### What is a Scope (component collection)?

A `Scope` is a dynamic collection of components. 

Scope stores and organizes your components. Bit Scopes works as a single remote location from which you can sync shared code throughout your projects.

You can imagine this collection as a music "playlist" of the code you share - you can create a collections for different projects, teams, themes and organizations while making individual components available to discover, consume and update for any team and for every projects. 

Scopes are cheap and easy to create in any level of abstraction, whether for specific project, idea or a group of people. You can create Scopes [on the cloud](https://bitsrc.io) or on any local server.

Benefits of organizing components in Scopes:

- **Dynamic** - Components can be dynamically added, modified or removed from Scopes.
- **Discoverable** - Each component becomes individually discoverable and available.
- **Curatable** - You can curate components on your Scopes, to control shared code quality and standards.
- **Predictable and available** - Component dependencies are being locked and cached in your Scope. Even if a component will be removed from its original Scope, your version of the component will keep on working.