---
id: how-dependency-management
title: Dependency Management
layout: docs
category: How Bit Works
permalink: docs/how-dependency-management.html
---
We designed Bit's dependency resolution and management for the use of code components. 
Its key features are:

* **Installation (import) performance** - In order to make component installation quick, we're resolving dependencies on export. This design approach makes component installation speed affected by bandwidth alone.
* **Predictable and deterministic** - In order to avoid dependency hell and dependency debugging, the same dependencies are installed exactly the same, every time.
* **Component availability** - Components are cached to make sure specific versions or components you've used are always available in your "closest Scope".

## On-export Dependency Resolution

### The benefits of on-export dependency resolution

* **Fast build time** - When your application builds, Bit does nothing but download a flat list of dependencies. That's it. Bit doesn’t have a dependency tree. Meaning there's no need for recalculating and downloading more dependencies. This means a faster build time.
* **Reliability** - Since every component is stored with its dependencies in a tested version, you can be sure it will keep working in any new environment (as long as someone has bothered to write tests...).
* **No duplications** - Bit knows which exact dependencies it needs to grab, so if there are any duplications, it will download a single working copy.

## Tags and dependencies

Unlike any other package or dependency manager, Bit locks the dependency graph when a component is tagged locally. Meaning that all the dependencies of the component are locked according to how they where configured when implemented and tagged.

Combined with the fact that Bit can isolate and test a single component before sharing it, it means that if a component had worked locally and was then tagged, Bit can re-create the exact same conditions in any remote workspace (Scope, project) that the component is imported to. On top of that, due to the fact that all dependencies are already resolved and locked, there are no round-trips to the server to get dependencies. With a single `import` command, a component and all its dependencies are downloaded as a single block.

## How does Bit resolve dependencies?

When a file is tracked by Bit as a component (or part of it), Bit reads its contents, and looks for any calls for external dependencies. For example, in JavaScript they can be `require` and `import`. Once all these calls are aggregated, Bit Determines whether the dependency is to another file within the project (but not within the component), or to an external library added by a Package Manager (npm, for example). For components with more than one file, Bit will iterate the process.

Once Bit holds a list of all the dependencies for the source codes it tracks for a specific component, it logs them in their right place; **Package dependencies** are logged into a file which is readable by the relevant Package Management tool (for JavaScript, a *package.json* file), alongside the version of each dependency, as stated in the project's package dependency list. Additional **file dependencies** are then calculated to see if any of these files is part of another component. If that's the case - Bit will add a **component dependency** for that component. In case the component requires a file that's part of the project, but not a part of any component, Bit will notify about it. This file has to be either added to the newly tracked component, or to be added as an additional component (more about this process in the [next section](#component-dependency)).

## Package Dependencies

All modern programming languages support some kind of *package dependencies*. Bit is able to read the implementation of the code component to figure out its package dependencies. Once tagged, all these dependencies are locked in place for the component, so when importing it to another project, which is lacking the package dependencies the component requires, it will be able to add them automatically to the consuming project, to ensure that the code will work.

When importing a component with package dependencies to a consumer project, Bit will check if the package dependencies are in place for that project workspace according to the specific development environment and programming language.  For example, in a NodeJS project, Bit will check if these dependencies are defined in the project's `package.json` file, and in the right SemVer range. It will also check the `node_modules` directory to see if they are installed.  Bit will then prompt a message showing the status, to let you know if any action is needed to ensure the component has all its dependencies in place.

## Component Dependency

Any component can be dependent on any other component, to form a *component dependency* graph. This will happen when projects start to heavily use Bit. When you're tracking a new component, which requires any file from a pre-existing component, Bit will register the pre-existing component as a dependency.

Bit also notifies you if your component depends on any additional files within your project. For example, you have a `common.js` file with many utility functions that a React component you're tracking is using. This `common.js` file is not a part of the component itself, but a `file dependency`.  In cases like this, you have two options:

1. Add the `common.js` file as a part of your component.
2. Create a new component from the `common.js` file, so Bit will add it as a component dependency to your original component.

Generally speaking, if there are several components that require the same `common.js` file, its better to track it as a component of its own, and have Bit set it as a dependency.
