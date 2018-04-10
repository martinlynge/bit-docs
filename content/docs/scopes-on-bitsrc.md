---
id: scopes-on-bitsrc
title: Scopes On Bitsrc
permalink: docs/scopes-on-bitsrc.html
layout: docs
prev: my-account.html
---

Scopes are distributed by nature. Setting up a free (forever for open source) remote Scope on [bitsrc.io](bitsrc.io) is quick and provides many advantages - but you can also choose to set up a remote Scope on your local machine.

## Scopes

Scopes are one of Bit's most important entities: they are your component collections.

Scopes functions as your source of truth, even when your components are sourced in synced in multiple repositories.

Scopes also act as a virtualized layer that allows you to manage your components across different projects and applications.

Creating a Scope on [bitsrc.io](bitsrc.io) is not only easier, it provides additional features for **discoverability** and **experience** such as a smart component search engine, component rendering, docs and examples parsed from the source code and an [isolated component environment](/docs/ext-concepts.html#what-is-an-isolated-component-environment) - so you can monitor and understand components without having to spend hours writing docs.

## Create a Scope on BitSrc

If you have an account on [bitsrc.io](bitsrc.io), simply visit your empty dashboard screen to create your first Scope. If not, [sign-up](https://bitsrc.io/signup) and create your first Scope.

Once signed-in, you can always click on the 'create new Scope' icon in the top-right corner of the screen.

You will now be prompted with the 'new Scope creation' window.

* Scope name - The name of the Scope
* Description - A short sentence describing the Scope and the components it contains (see [example](https://bitsrc.io/bit/utils#array)).
* Visibility - Determines who can view the Scope.
* Public - Free Scope to be searched and viewed by the whole BitSrc community. BitSrc offers free hosting for public open source Scopes, now and forever.
* Private - Closed Scope. Only members can search it, and view its contents.
* License - Code license for all the contents of the Scope.

Once all of the details are filled in, click ‘create Scope’.

You will then be redirected to your newly created Scope.

## Update Scope settings

### How to update Scope settings

1. Click on "settings" inside your Scope.
2. Change your description.
3. Click on "Update Scope".

#### Update visibility

You can change the scope visibility from private to public, via the Scope settings page.  
Changing a Scope visibility will affect all the components within that Scope, so be careful when doing so, as this will affect other developers using the components.

If a component is changed from public to private, all other Scopes that had it in their cache, will still keep it in the last public version of it. However, all other projects that installed a component via NPM/Yarn, and do not have specific permission set to view the private component, will no longer have access to them.

#### Scope licenses

Currently there are 5 licenses options you can choose from:

* None
* GPL- 3.0
* MIT
* Apache-2.0
* BSD (1,2,3)

If you chose one license and you want to change it, click on "Settings" inside the specific Scope and change the license.

Important:
The license will be applied to all the components within the Scope.

## Scope permissions

### Permission types

There are 3 type of permissions that can be granted for your BitSrc Scopes:

* Owner - an owner has full root access to the Scope. They have all permissions, including the ability to set permissions and manage Scope membership.
* Collaborator - a collaborator can contribute to a Scope. They can't invite new collaborators.
* Viewer - a viewer can only view the Scope and its contents, without changing anything in it.

### Adding collaborators

A Scope owner can add an unlimited amount of signed-up users to have 'collaborator' permissions on any Scope the owner owns.

To add a collaborator:

1. Go to your "Scope" page.
2. Type either the username or email address of the collaborator you wish to add inside the "add collaborator" text Scope.
3. Click on "Add".

### Removing collaborators

To remove a collaborator:

1. Go to your "Scope" page.
2. Find the user you would like to remove in the Collaborators list.
3. Hover over the username and a "Remove" button will appear next to it.
4. Click on "Remove" to remove the relevant collaborator.

Note: removing people might hurt their feelings :)

## The Scope Page

The Scope page ([example](https://bitsrc.io/bit/movie-app)) is a useful UI for working with Scopes hosted on BitSrc.

They present valuable information about the Scope such as:

1. Scope Name
2. Popularity
3. License
4. Collaborators
5. etc...

The Scope page also offers an overview of the different components found in this Scope, and enables you to filter them by namespaces and more.

Note that the pane to the left of the components offers quick browsing of the components based on their namespaces. This helps users find components by context and functionality.
