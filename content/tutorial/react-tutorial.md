---
id: react-tutorial
title: Sharing React Components with Bit
permalink: tutorial/react-tutorial.html
---

Bit enables you to share and sync source code components between different SCM repositories and projects.
React is known for its ease in separation and reuse of components, and as such, it's perfect for combining with Bit.

Here is [an example](https://bitsrc.io/bit/movie-app) of a React [Scope](/docs/scopes-on-bitsrc.html#scopes) of individually available React components shared from this [GitHub library](https://github.com/itaymendel/movie-app). These components are ready to be installed and updated in different projects.

## Before We Start

### In this tutorial you'll learn

- How to share a React component from one project's repository to a different one using Bit
- How to integrate building tools into a React component
- How to export a React component to a remote [Bit Scope](/docs/scopes-on-bitsrc.html#scopes)
- How to update a React component using Bit
- How to render a React component for preview

We have created an [example project](https://github.com/teambit/tutorial-react) containing two directories, both created with [create-react-app](https://github.com/facebookincubator/create-react-app).

For this tutorial, each directory represents an individual project.
We would share a single React component between these "projects".

The first project, called **export-button**, contains a React `button.js` component.
Using Bit, you will share this component and use it in the second project called **import-button**.

> **Note**
>
> [bitsrc.io](https://bitsrc.io) is the free community hub for Bit. You can host and manage your components and Scope permissions, view useful information such as docs, test results and live rendering for React components.
It's free for open source, and always will be.

### Prerequisites

We assume familiarity with JavaScript and React.

Before starting this tutorial, please [Install Bit](/docs/installing-bit.html) on your computer.
If you have already installed Bit, please verify the installation by running the following command:

```bash
$ bit --version
```

> **Tip!** 
>
> We recommend that after you installed Bit on your computer to sign up and set up an SSH authentication with [bitsrc.io](https://bitsrc.io). Please see [Set up SSH authentication to bitsrc.io](/docs/setup-authentication.html#set-up-bitsrcio-connectivity)

## Getting Started

In order to follow this tutorial, clone the tutorial project by using Terminal:

```bash
$ git clone https://github.com/teambit/tutorial-react.git
$ cd tutorial-react
```

We'll use this example project to show you how to work with Bit.

## Add, Export & Share Components

In the this section, you'll use a React component and share it ([export](/docs/cli-export.html)) to a remote [Scope](/docs/scopes-on-bitsrc.html#scopes) on [bitsrc.io](https://bitsrc.io).

### Initializing Bit on your project

In the terminal, navigate to `export-button` project directory and `npm install`:

```bash
$ cd export-button
$ npm install # or yarn install
```

Now initialize Bit:

```bash
$ bit init

successfully initialized an empty bit workspace.
```

Congrats! You've just made your first step using Bit. You can now see responses from Bit in your terminal.

> **Note**
>
> Bit has just created some new stuff in the project’s root. The [.bit](/docs/initializing-bit.html#component-store) directory stores all the information and binding that Bit does in the background, and [bit.json](/docs/conf-bit-json.html) stores the configuration for your environments, dependencies, and the location to store them all.
To learn more see [Initializing Bit](/docs/initializing-bit.html)

### Add & Track with Bit

It’s time to start [tracking](/docs/cli-tag.html) your React components.
Start by [adding](/docs/cli-add.html) the button component.
Let’s give it a name, in this tutorial we called it `ui/button`:

```bash
$ bit add src/ui/button.js --id ui/button

tracking component ui/button:
added src/ui/button.js
```

Now that your React component is tracked by Bit, you can check the status of your workspace by using [bit status](/docs/cli-status.html).

```bash
$ bit status

new components
     > ui/button... ok

----------------

no modified components

----------------

no staged components

----------------

no auto-tag pending components

----------------

no deleted components

```

You can see that the `ui/button` component was just added to the “new components” section.

Use the [bit status](/docs/cli-status.html) command as often as you’d like to see the current status of all the components you are working on. When in doubt - `bit status`!

### Add Build environment

#### Why & What are Build environments

A React component needs to be compiled so it can run on a browser.
**Build** environment is a 'build task` that Bit uses to run and compile the component. To learn more, see [build components](/docs/building-components.html).

A *built* component can then be [consumed as a node module](/docs/installing-components-using-package-managers.html) directly from [bitsrc.io](https://bitsrc.io) using NPM or Yarn.

The React component you are using needs to be compiled, for this tutorial we'll use a **bundler**. A bundler is a type of a compiler.

Bit has multiple build environments and bundlers available for your use, hosted as Bit components in a Scope called ["envs"](https://bitsrc.io/bit/envs).

> **Note: Isolated Environment**
>
> Bit creates an isolated environment for each component in your scope. Bit runs all the component's build tasks there. This will also happen after exporting components to a remote scope. To learn more see [Isolated Component Environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment)

### Bundle your component

Add an environment by using the [Import](/docs/cli-import.html#import-a-single-component-from-a-remote-scope) command suffixed with the relevant flag. This will ensure all components in your local scope will have the same  environment.

```bash
$ bit import bit.envs/compilers/react -c

the following component environments were installed
- bit.envs/compilers/react@0.0.13
```

### Build your component

To build the `ui/button` component, run the bit build command:

```bash
$ bit build ui/button

/Users/../tutorial-react/export-button/dist/src/ui/button.js
```

### Lock your React component version

Bit has the ability to [lock your components](/docs/versioning-tracked-components.html) state, by using the [bit tag](/docs/cli-tag.html) command (similar to [git tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging)). Tagging a component will lock the dependencies of the component, create a version and prepare it for export.

If you like it, better put a tag on it!

```bash
$ bit tag ui/button

1 components tagged | 1 added, 0 changed, 0 auto-tagged
added components:  ui/button@0.0.1
```

After tagging your component, you can check your status:

```bash
$ bit status

no new components

-----------------

no modified components

-----------------

staged components
     > ui/button. versions: 0.0.1... ok

-----------------

no auto-tag pending components

-----------------

no deleted components
```

You can see that the component is now in the staged area. The components in this area are version locked and ready for export. 
Before you continue, check that you have a [Scope](/docs/scopes-on-bitsrc.html#scopes) ready for the new component you are about to export.

### Create a Scope on [bitsrc.io](https://bitsrc.io)

In order to export a component, you need to specify a [Scope](/docs/scopes-on-bitsrc.html#scopes) to export your component to.

A [Scope](/docs/scopes-on-bitsrc.html#scopes) is a remote directory that stores, components that you create in order to work, share and track your components. Other developers can then access your public Scope and use your components. You can view a scope example in the [tutorial project Scope](https://bitsrc.io/odedreuveny/example).

 To create your own Scope follow [these instructions](/docs/quick-start.html#create-a-scope).

### Export your React component to [bitsrc.io](https://bitsrc.io)

You can share components from your project to a Scope by using the `bit export` command.
In this example, our [bitsrc.io](https://bitsrc.io) username is [wonderwoman](https://bitsrc.io/wonderwoman) and the scope is [diana](https://bitsrc.io/wonderwoman/diana):

```bash
$ bit export wonderwoman.diana

exported 1 components to scope wonderwoman.diana
```

Once the you exported the component, you can see it on your Scope on [bitsrc.io](https://bitsrc.io).

If you [check status](/docs/cli-status.html), you can see the component is not displayed.
Bit has created a copy of the component, along with all its dependencies, the component is now hosted in your remote Scope.

```bash
$ bit status

no new components

-----------------

no modified components

-----------------

no staged components

-----------------

no auto-tag pending components

-----------------

no deleted components
```

Go to your [bitsrc.io](https://bitsrc.io) profile to view  your Scope and the exported component. Your Scope should look like [this](https://bitsrc.io/odedreuveny/example/ui/button). Your component is now available for other developers to import and use freely.

## Install & Use Components

#### Consume Components Using Package Managers

Learn more about [Installing Components using Package Managers](/docs/installing-components-using-package-managers.html)

### Install your React component

We’ve created a bare [create-react-app](https://github.com/facebookincubator/create-react-app) project for you inside the repo called `import-button`. This is where you're going to consume your newly exported React component.

In your terminal, navigate to the `import-Button` project directory and `npm install`:

```bash
$ cd ../import-button
$ npm install # or yarn install
```

### Configure [bitsrc.io](https://bitsrc.io) as a scoped registry

To configure npm (or yarn) installing Bit components hosted on [bitsrc.io](https://bitsrc.io), you will need to configure [bitsrc.io](https://bitsrc.io) as a [Scoped Registry](https://docs.npmjs.com/misc/registry)

```bash
$ npm config set @bit:registry https://node.bitsrc.io
```

Now you can use npm or yarn in order to install your components.

```bash
$ npm install @bit/wonderwoman.diana.ui.button
```

### Use in your project

In your code editor open the `src/App.js` file and update  the `Import` statement:

```js
import Button from '@bit/wonderwoman.diana.ui.button';
```

Call the node module:

```js
<Button />
```

Your `src/App.js` should look like this:

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Button from '@bit/wonderwoman.diana.ui.button';

class App extends Component {
 render() {
   return (
     <div className="App">
       <header className="App-header">
         <img src={logo} className="App-logo" alt="logo" />
         <h1 className="App-title">Welcome to React</h1>
       </header>
       <p className="App-intro">
         To get started, edit <code>src/App.js</code> and save to reload.
       </p>
       <Button />
     </div>
   );
 }
}

export default App;
```

Run your project:

```bash
$ npm start
```

You can see the button component rendered in your project.

## Modify Components

You can modify the imported React component (`ui/button`) in the consuming project located locally (in this tutorial the `import-button` project), and export it back to the remote scope.

### Initialize Bit in the consuming project

Bit allows developers to source components into other projects and to either use these components or modify them according to their needs. Sourcing components is done with the [bit import](/docs/cli-import.html) command.

In this tutorial the consuming project is the`import-button` project. Initialize Bit in it.

```bash
$ bit init

successfully initialized an empty bit workspace.
```

### Import the component to your project

Use the `bit import` command to source your component to the project.

```bash
$ bit import wonderwoman.diana/ui/button --path src/components/ui/button

successfully ran npm install at /Users/../tutorial-react/import-button/src/components

successfully imported one component
- wonderwoman.diana/ui/button@0.0.1
```

Run `npm start` to see that the project renders just the same.

```bash
$ npm start
```

Open your code editor to view the updated source tree. In your tree you can see a new directory `src\components`, this directory holds the sourced component within its isolated component environment.

> **Configurations for Imported Components**
>
> To configure the way your project  handles imported component's `dist` files, dependencies and packages, please see [here](https://docs.bitsrc.io/docs/conf-bit-json.html).

#### How Does it Work

After installing a component using npm, use the `bit import`command in your code. Bit identifies the imported component as a `bit component` and replace the component's node module with a symlink to the location of the sourced component.

```bash
$ tree node_modules/@bit

node_modules/@bit
└── wonderwoman.diana.ui.button -> /Users/../tutorial-react/import-button/src/components
1 directory, 0 files
```

Your `import` statement now refers to the sourced component. Every time you use `bit import` in a project, Bit will create a symlink to the component in your node modules directory. You can use absolute `import` statement to use sourced components.

### Modify an imported component

Update the `button.js` component from the **consuming** project (e.g `import-button`) and [export](/docs/cli-export.html) the changed component back to the remote Scope you've created.

Open your IDE and navigate to the file `src/components/button.js`, update the default properties section of the button to:

```js
Button.defaultProps = {
  text: 'Go Diana',
  buttonColor: "rgba(161, 188, 202, 0.34)",
  buttonHoverColor: "rgb(119, 148, 162)"
}
```

In your terminal run:

```bash
npm start
```

> **Note**
>
>Changes are not updated on your browser, yet...

#### Build your "dist" files to reflect the changes in your source code

When modifying an imported component with a build environment, changes you make are not immediately updated, since Bit links your code to the component's `dist` directory and **not** to the source code. To update the component's `dist` rebuild the component using the [bit build](/docs/cli-build.html) command:

```bash
$ bit build

successfully installed the bit.envs/compilers/react@0.0.13 compiler
wonderwoman.diana/ui/button@0.0.1
/Users/../tutorial-react/import-button/src/components/dist/button.js.map
/Users/../tutorial-react/import-button/src/components/dist/button.js
```

Now run:

```bash
$ npm start
```

Take a look at your browser and see the changes, as expected!

### Lock version & export to [bitsrc.io](https://bitsrc.io)

Now that you have a modification in the source of the component, run the `status` command again, to see that Bit changes the component's state.

```bash
$ bit status

no new components

-----------------

modified components
     > ui/button ... ok

-----------------

no staged components

-----------------

no auto-tag pending components

-----------------

no deleted components
```

A simple `bit tag` command will lock a new version for the component with this change.

```bash
$ bit tag --all

1 components tagged | 0 added, 1 changed, 0 auto-tagged
changed components:  ui/button@0.0.2
```

### Export component new version

To export the component new version, use the `bit export` command again, but now add the flag `--eject`. This flag allows  Bit to replace the component node module with the exported version, so that your project will have less code and clutter.

```bash
$ bit export wonderwoman.diana ui/button --eject

exported 1 components to scope wonderwoman.diana
```

Open your IDE and see that the component is no longer a part of it, and was added to your project's `package.json` file.

 Check your Scope in [bitsrc.io](https://bitsrc.io), you can see that there is a new version for the React component.

## Import updated component

Now you can use the updated React component, and import it in to the `export-button` project. By doing so, you will receive the updates exported to the remote Scope into your local project as well.

### Consume updated component

In your terminal, go to the `export-button` project:

```bash
$ cd ../export-button
```

Now update your component by re-importing:

```bash
$ bit import wonderwoman.diana/ui/button --force

successfully imported one component
- wonderwoman.diana/ui/button@0.0.2
```

Run the project again, and see that it is renders the updated button:

```bash
$ npm start
```

## Wrapping up

In this tutorial you’ve learned how to:

- Share a React component between projects with the [export](/docs/cli-export.html) & [bit import](/docs/cli-import.html)  commands
- Add a [build](/docs/building-components.html) task to a React component
- Update components in your remote [Scope](/docs/scopes-on-bitsrc.html#scopes) on [bitsrc.io](https://bitsrc.io)
- Install a component using a [Package Managers](/docs/installing-components-using-package-managers.html)
- Source a component so you can update it in different project
  - Change a React component from the consuming project
  - Build a component to reflect source code changes
  - Export a component to update the remote [Scope](/docs/scopes-on-bitsrc.html#scopes) on [bitsrc.io](https://bitsrc.io)
- Update a source component using Bit

**Great job, you rock (& Roll)!**