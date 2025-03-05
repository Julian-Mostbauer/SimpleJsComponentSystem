# Simple JS Component System

## About

This is a simple js component system,
which allows you to create and use these components in html pages.

Example taken from [Linux Ricing Guide](https://github.com/Julian-Mostbauer/LinuxRicingGuide), the project where this system was originaly used in.

## Usage

To use the components in your HTML page, follow these steps:

1. **Include the Component Scripts**: Add the component scripts to your HTML file within the `<head>` tag.

```html
<script async type="module" src="components/card-big-image.js"></script>
<script async type="module" src="components/navbar.js"></script>
```

2. **Add the Component Tags**: Use the custom component tags in your HTML body.

```html
<navbar></navbar>
<card-big-image
  data-image-path="../assets/img/card-distributions.webp"
  data-title="Distributions"
  data-description="List of some linux distributions you can use. These contain common but also some exotic ones."
  data-link="distros.html"
></card-big-image>
```

The props need to be prefixed with `data-` to be recognized.

## Making a component

### Basic structure

```js
import ComponentBuilder from "./path/to/component-builder.js";

// the name the system will use to call the component with
const componentName = "card-big-image";

// the code that will be used by the component
const code = `
  <div class="card">
    <img src="{{image-path}}" alt="{{title}}">
    <div class="card-content">
      <h2>{{title}}</h2>
      <p>{{description}}</p>
      <a href="{{link}}">Learn more</a>
    </div>
  </div>
`;

const cardBigImageBuilder = new ComponentBuilder(componentName, code);

cardBigImageBuilder.build();
```

You can give props to your components implicitly, by using them in the code. The props can be used by wrapping their name in in double braces like this`{{ name }}`.

They can also be explicitely added by calling `addAdditionalProps` like this:

```js
exampleBuilder.addAdditionalProps(["prop1", "prop2", "prop3"]);
```

and giving an array of strings, which will be the names of the props.
This needs to be down if you are only using the prop inside of the onMount.

## OnMount

By calling `setOnMount` you can an a function that gets called right after the component gets placed. The OnMount has access to the props and can be async.

```js
const onMount = async (props) => {
  const data = await fetch(...)
  console.log(`${props["user-name"]}: ${data.msg}`)
}

exampleBuilder.setOnMount(onMount)
```
