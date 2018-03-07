---
id: documenting-components
title: Documenting Components
permalink: docs/documenting-components.html
layout: docs
category: Getting Started
prev: removing-components.html 
---

Documentation is the first step in increasing code discoverability and reusability by allowing developers to understand how to use a component within their code.

## Different types of component documentation

Bit supports several methods of documenting components, each with its own reasoning and functionality.  
You can use any of the methods of documentation, none of them, or a combination.

## Documenting a component using a Markdown file

The simplest method of documentation is to attach an external file to the component which holds its documentation.  
Bit natively supports [Markdown](https://en.wikipedia.org/wiki/Markdown) format for such task.

When you are tracking files for a component, you can add any Markdown file to it.  
When exported to [bitsrc.io](https://bitsrc.io/), the component's overview tab will render that Markdown file found within the component.

> **Note**
>
> The Markdown file you are using can have any name, not just README.md.  
This is useful for when for you have several Markdown files within the same directory, and each file refers to a different component.

## Documenting a component with JSDocs

[JSDocs](http://usejsdoc.org) is a standard format for documenting APIs in JavaScript.  
If you use JSDocs to document APIs in your components, Bit will read and parse them to preset them in the component overview tab on [bitsrc.io](https://bitsrc.io/).

Here is an example for the usage of JSDocs in a component with the [rendered result](https://bitsrc.io/bit/utils/array/diff).

```js
/** @flow */

/**
 * Computes the difference between two array references.
 * @name diff
 * @param {[]} firstArray
 * @param {[]} secondArray
 * @returns {[]} returns an array representing the difference between the two arrays
 * @example
 *  diff([1,2,3], [1,2,3,4,5]) // => [4,5]
 */
export default function diff(firstArray: any[], secondArray: any[]): any[] {
  return firstArray.concat(secondArray).filter((val) => {
    return !(firstArray.includes(val) && secondArray.includes(val));
  });
};
```

## Documenting a component with Markdown in a code comment

Another method of documenting components is to simply use a JSDocs comment block to write the docs.  
When exporting the component, Bit will use the text it finds there and present it in the component's overview tab.
You can also use Markdown annotations to add additional features to the documentation (such as examples).

```js
/** @flow */

/**
 * # diff
 *
 * Computes the difference between two array references.
 *
 * ## Params
 *
 * - firstArray
 * - secondArray
 *
 * ## returns
 *
 * An array representing the difference between the two arrays
 *
 * ## example
 *
 * ```js
 *  diff([1,2,3], [1,2,3,4,5]) // => [4,5]
 * ```
 */
export default function diff(firstArray: any[], secondArray: any[]): any[] {
  return firstArray.concat(secondArray).filter((val) => {
    return !(firstArray.includes(val) && secondArray.includes(val));
  });
};
```