---
id: ext-isolated-component-environment
title: Isolated Component Environment
permalink: docs/ext-isolated-component-environment.html
layout: docs
category: Extending Bit
---

Creating an isolated component environment for a components allows Bit to run tasks such as test/build outside the context of a project, thus ensuring a component is completely decoupled from any project.

When you manually copy and paste a piece of functionality from one project to another, you can't ignore the fact that the piece of functionality lives within the project, and thus has its own context. That context can be defined by the packages it uses, other files it requires, etc. In order to make the few lines of code usable on another project, one will need to provide all the missing dependencies.

Bit solves most of the problem - all the dependencies and external files are calculated when a component is being tracked and tagged.

So this means that if we are to take all the dependencies, and put them in a single place for the tracked Bit component to use, we can create an **isolated component environment** outside the context of *any* project.

The main benefit behind complete component isolation is the ability to validate that Bit can recreate the entire component environment on any other project, without affecting its functionality. 
In addition, Running extensions on components, without a project, allows actions such as building, testing, generating docs and even bundling individual components and publishing them to external package managers.

## How does it work?

When creating an *isolated component environment*, Bit imports the component to a separate directory, along with its dependencies. This creates a clean environment in which Bit can perform actions on the component without the 'noise' of the rest of the project.

## Isolated component environment and extensions

[Extensions](/docs/ext-concepts.html) can utilize the *isolated component environment* for their own needs. Just imagine how many custom actions require an *isolated component environment* - pack, publish, lint...

Want to learn how? head over to the [extensions development](/docs/ext-developing-extensions.html#creating-an-isolated-environment) section.