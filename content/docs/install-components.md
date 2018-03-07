---
id: install-components
title: Installing components
permalink: docs/install-components.html
layout: docs
category: Troubleshooting
prev: authentication-issues
next: clearing-bit-cache
---

There are several issues you may encounter when you install a component from [bitsrc.io](bitsrc.io) using NPM/Yarn.

## Getting unauthorized (401) when installing a component

### Not sufficient permissions to the remote scope

You do not have the right permissions on the scope that the components are hosted in, and unable to access its components.

To resolve this issue contact the Scope admin, and request for Read permissions.

### Yarn does not send the authentication token with the install command

If you have the required permission to install components from a scope, and you are pulling a repository which has a committed yarn.lock file, and the installation of components returns an error saying it is 'unauthorized' (HTTP 401), this means that you encounter an [open issue](https://github.com/yarnpkg/yarn/issues/4451) that causes Yarn not to send its authentication token when installing packages from a `yarn.lock` file.

To resolve this set the always-auth parameter to true in the project's `.npmrc`. This will force the authentication token to each package installation.