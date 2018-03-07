---
id: testing-components
title: Testing Components
permalink: docs/testing-components.html
layout: docs
category: Getting Started
prev: building-components.html
next: removing-components.html
---

Bit can run the component's tests in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).

The testing process of bit components is done in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment), using [testers](/docs/ext-testing.html), which are environments - a special kind of [extension](/docs/ext-concepts.html#extensions-vs-environments).

## Executing components tests

In order to test a component, you need to have a tester defined.

### Defining a tester for your project

A tester is defined per [Scope](/docs/what-is-bit.html#what-is-a-scope-collection), and once defined, testing a component within that Scope will test the component using the defined tester.
In order to do that, just [import the tester](/docs/cli-import.html#import-a-new-environment):

```bash
$ bit import bit.envs/testers/mocha --tester
the following component environments were installed
- bit.envs/testers/mocha@0.0.7
```

You can now go take a look at the `env` configuration in your [bit.json](/docs/conf-bit-json.html#env--object) and see your tester over there.

#### Curated list of test environments on Bitsrc.io

You can find a list of Test Environments maintained by the [bitsrc.io](bitsrc.io) team [here](https://bitsrc.io/bit/envs).

- [Mocha](https://bitsrc.io/bit/envs/testers/mocha).
- [Jest](https://bitsrc.io/bit/envs/testers/jest).

### Adding test/spec files to your components

Bit is not going to guess which files are your spec files, right? You should [track your spec files correctly](/docs/cli-add.html#track-a-component-with-a-test-file).

### Test your component

Once you have a defined tester, you can [test your component](/docs/cli-test.html):

```bash
$ bit test foo/bar
foo/bar
tests passed
file: dist/test/bar.test.js
total duration - 4ms

✔   bar should do nothing - 1ms
```

> **Note**
>
> If you have a [compiler](/docs/building-components.html) defined, testing a component will also cause Bit to [build](/docs/cli-build.html) it first.

## Executing component tests remotely

When staging or exporting components, Bit runs the components' extensions in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment). So the `test` process for the component will run outside the context of the project. This allows Bit to make sure the component is truly isolated.

### Self-managed Scope

After exporting a component to a [self-managed remote Scope](/docs/organizing-components-in-scopes.html#self-managed-scope), the component will be [built](/docs/building-components.html) and then tested in the remote Scope in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).

### bitsrc.io-managed Scope

After exporting a component to a [bitsrc.io managed Scope](/docs/organizing-components-in-scopes.html#creating-a-scope-on-bitsrcio), [bitsrc.io](bitsrc.io) will create an isolated container, where the component will be [built](/docs/building-components.html) and then tested in the remote Scope in an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment).
When the tests have finished running, their results will be displayed as part of the component's documentation on [bitsrc.io](https://bitsrc.io).
As the Scope [owner or collaborator](/docs/scopes-on-bitsrc.html#Scope-permissions), you can enter the component page on bitsrc.io, and view the CI results in the `Console Output` tab.

## Implementing a tester

You can implement your own custom tester. [See here](/docs/ext-testing.html) for more details.