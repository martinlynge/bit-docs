---
id: install-components
title: Installing components
permalink: docs/install-components.html
layout: docs
category: Troubleshooting
prev: authentication-issues.html
next: clearing-bit-cache.html
---

There are several issues you may encounter when you install or import a component from [bitsrc.io](bitsrc.io).

## NPM or Yarn throws 'package not found' when importing a component

If you are importing a component using `bit import`, and you get a message similar to these:

**NPM**

```bash 
failed running npm install at /Users/iteymendel/devenv/example-npm-error/components/utils/string/pad-left
npm ERR! code E404
npm ERR! 404 Not Found: @bit/bit.utils.string.pad-left@0.0.1
```

**Yarn**

```bash
failed running yarn install at /Users/iteymendel/devenv/example-npm-error/components/utils/string/pad-left
error An unexpected error occurred: "https://registry.yarnpkg.com/@bit%2fbit.utils.string.pad-left: Not found".
```

This means that when a component you are importing from [bitsrc.io](bitsrc.io) has other component dependencies, by default Bit will try to install the dependencies as node modules, using NPM or Yarn. This means that your package manager needs to have `@bit` defined as a scoped registry, so it can install packages from there.

To do so, run the following command and use your [bitsrc.io](bitsrc.io) credentials.

```bash
npm login --registry=https://node.bitsrc.io --scope=@bit
```

Read more about this feature [here](https://docs.bitsrc.io/docs/installing-components-using-package-managers.html).


## Getting unauthorized (401) when installing a component

### Not sufficient permissions to the remote scope

You do not have the right permissions on the scope that the components are hosted in, and unable to access its components.

To resolve this issue contact the Scope admin, and request for Read permissions.

### Yarn does not send the authentication token with the install command

If you have the required permission to install components from a scope, and you are pulling a repository which has a committed yarn.lock file, and the installation of components returns an error saying it is 'unauthorized' (HTTP 401), this means that you encounter an [open issue](https://github.com/yarnpkg/yarn/issues/4451) that causes Yarn not to send its authentication token when installing packages from a `yarn.lock` file.

To resolve this set the always-auth parameter to true in the project's `.npmrc`. This will force the authentication token to each package installation.