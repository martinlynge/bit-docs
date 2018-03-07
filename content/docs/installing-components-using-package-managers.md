---
id: installing-components-using-package-managers
title: Installing Components using Package Managers
permalink: docs/installing-components-using-package-managers.html
layout: docs
category: Quick Start
prev: organizing-components-in-scopes.html
next: importing-components.html
---

Bit components can be installed with any package manager that implements the CommonJS package registry specification. Popular examples would be Yarn and NPM.

Once Bit components are [organized in remote Scopes](/docs/organizing-components-in-scopes.html), they can be installed with any package manager client that implements the CommonJS package registry specification such as [Yarn](https://yarnpkg.org) or [NPM](https://npmjs.org).

For example, you can check out our [movie-app demo Scope](https://bitsrc.io/bit/movie-app) and install any component from that Scope components using NPM or Yarn.

```bash
yarn add @bit/bit.movie-app.components.hero
```

## Configuring Bit as a registry in NPM and Yarn clients

Bit can be configured as a registry with any CommonJS compatible client such as NPM and Yarn.

### Configuring Bit as a scoped registry

Bit can be configured as a [scoped registry](https://docs.npmjs.com/misc/scope#associating-a-scope-with-a-registry) and can be associated with any scope name.

```bash
npm config set @bit:registry https://node.bitsrc.io
```

To install private components from [bitsrc.io](https://bitsrc.io), use `npm login` to associate Bit with an authenticated scope. Use your Bit credentials to login.

```bash
npm login --registry=https://node.bitsrc.io --scope=@bit
```

To learn more about NPM scoped registries, please refer to [NPM's documentation](https://docs.npmjs.com/misc/scope#associating-a-scope-with-a-registry). 

### Configuring Bit as a proxy registry

For better user experience, Bit can be configured as a proxy registry in Yarn and NPM. Bit will proxy any request to the NPM official registry.

```bash
npm config set registry https://node.bitsrc.io
```

## Package naming convention

Package naming convention includes the Bit owner, Scope name, namespace and the component name.

An example for installing a component using Yarn or NPM would be as following:

```bash
yarn add <owner>.<scope>.<namespace>.<component-name>
```
