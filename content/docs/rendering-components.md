---
id: rendering-components
title: Rendering Components
permalink: docs/rendering-components.html
layout: docs
category: Getting Started
prev: testing-components.html
next: removing-components.html
---

To allow users to interact directly with your components available [bitsrc.io](https://bitsrc.io), render your component, as shown in this doc.

See the rendered component [here](https://bitsrc.io/bit/movie-app/components/hero).

## How Does it Work

- Add annotations to your component, documenting how it should be rendered.
- Bundle your component as a [UMD](https://github.com/umdjs/umd), using one of Bit’s bundlers. See  [Building Components](docs/building-components.html) .
- [Tag](/docs/cli-tag.html) & [Export](/docs/cli-export.html) your component.
- Your component will be available for preview at bitsrc.io [Scope](/docs/scopes-on-bitsrc.html#scopes).

### Annotations

Some [JSDocs](http://usejsdoc.org/about-getting-started.html#adding-documentation-comments-to-your-code) annotations needs to be added to your component, as shown:
```js
/**
* @render react
* @name Hero
* @example
* <Hero
*   title="Season 66 will be available soon!"
*   description="Lorem ipsum dolor sit amet hey! id quam sapiente unde voluptatum alias vero debitis, magnam quis quod."
*/
class Hero() {
    ...
}
```

Use the following  annotations:

### @render react
Specify the renderer for the component. Currently only "`react`" is supported.

> **Bit Renderers**
>
> Renderers are Bit component, bitsrc.io supports a variety of platforms using renderers. Additional platforms support is coming soon...

### @name <NAME>
Specify the component name, so it will work correctly when the code is minified.

### @example
Specify how to render the component.

## Bundles

To render your component, it needs to be bundled as a [UMD](https://github.com/umdjs/umd).
`UMD` is a Javascript standard that works in the browser, in Nodejs, and in your workspace.

### Bundling components

To bundle your component, use [bundler](https://bitsrc.io/bit/envs/bundlers/webpack). A bundler is a type of a  [compiler](https://docs.bitsrc.io/docs/cli-build.html). You can use the Bit bundlers available at the [Bit envs Scope](https://bitsrc.io/bit/envs/). Currently the bundler coms in 2 configurations, [with](https://bitsrc.io/bit/envs/bundlers/webpack) & [without](https://bitsrc.io/bit/envs/bundlers/webpack-css-modules) css modules. You can extend or modify it to your needs, see more [here](https://docs.bitsrc.io/docs/cli-build.html).

Bit bundles the components as isolated environments **without** the component’s dependencies (hurray!).
If other Bit components are used as dependencies, these components should be consumed from a [package manager](/docs/installing-components-using-package-managers.html) and not as relative dependencies.
Currently when you consume a component relatively, it will be bundled as part of the same file.
Support for relative dependencies will be added in the near future. 

For example, let’s take a look at our [movie-app repository](https://github.com/teambit/movie-app),  and its corresponding [components](https://bitsrc.io/bit/movie-app/). The **Hero** component depends on the **HeroButton** component.
When you consume the **HeroButton** component relatively, it will be bundled along with the **Hero** component:

```js
$ import HeroButton from '../hero-button';
```

However, when you consume the **HeroButton** component from the package manger, the **Hero** component will be bundled without it, which is what you want:

```bash 
$ import HeroButton from '@bit/bit.movie-app.components.hero-button';
```

### Adding a bundler to an existing component 

- **An imported component** - [import](/docs/updating-sourced-components.html) the component using the `--conf` flag, so the component will have its [bit.json](/docs.bitsrc.io/docs/conf-bit-json.html) file. At `bit.json` you can set a compiler.

```bash
$ bit import bit.movie-app/components/hero-button --conf
```

Open your IDE and set the compiler id in your *component* bit.json file:

```js 
"env": {
    "compiler": "bit.envs/bundlers/webpack@0.0.6",
    "tester": "bit.envs/testers/karma-mocha-react@0.0.18"
}
```

- **Authored component** - an authored component is a component which is a part of a full project.
[Import the compiler](/docs/building-components.html#defining-a-default-compiler-for-your-project) to your project.

```bash
$ bit import bit.envs/bundlers/webpack -c
```

## Injecting Dependencies

Bitsrc.io  aspires to provide the component with all the decencies needed for preview.
In a case of an error, it will be displayed at the preview frame. Errors almost always happen because there was a problem in the bundle itself.

## Troubleshooting

When the preview is not working try the following:
- Open dev console and look for errors.
- Verify the bundle work by importing it locally, from another file.
- Manually check that the bundling worked - check out the `dist` directory, see that the file is formatted correctly.