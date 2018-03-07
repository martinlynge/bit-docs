---
id: common-errors
title: Common Errors
permalink: docs/common-errors.html
layout: docs
category: Troubleshooting
prev: clearing-bit-cache.html
---

TODO

* no \'\' for `bit add -t` - corrupt bit.json,status - windows; test/{FILE_NAME}.spec.js was not found - linux; 
* no \'\' for `bit add -e` - not excluding;
* space in path
* setting ssh key to "~/.ssh/bit_private_key_rsa"; fix - bit config set ssh_key_file /Users/<username>/.ssh/bit_private_key_rsa; error message - Could not authenticate
Please make sure you have configured ssh access and permissions to the remote scope - on bit commands.
* tagging a component that depends on a new component
* module not found - when testing and a dependency is not built
