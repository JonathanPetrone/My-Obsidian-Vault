## Basic boilerplate

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML 5 Boilerplate</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
	<script src="index.js"></script>
  </body>
</html>
```

## HTML Tags and explanations:
- Use `<section>` to separate content with HTML
- The `<figure>` element represents self-contained content and will allow you to associate an image with a caption.
- Like so: `
	<figure>
          <img src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/lasagna.jpg" alt="A slice of lasagna on a plate.">
          <figcaption>Cats love lasagna.</figcaption>
	</figure>`
- 'em'  makes text cursive and emphasized
- 'strong' makes text bold
- The `action` attribute indicates where form data should be sent. For example, `<form action="/submit-url"></form>` tells the browser that the form data should be sent to the path `/submit-url`.
- The `label` elements are used to help associate the text for an `input` element with the `input` element itself (especially for assistive technologies like screen readers).
- The `fieldset` element is used to group related inputs and labels together in a web form. `fieldset` elements are block-level elements, meaning that they appear on a new line.
- The `legend` element acts as a caption for the content in the `fieldset` element. It gives users context about what they should enter into that part of the form.
- The `title` element determines what browsers show in the title bar or tab for the page.

## CSS:

In CSS opacity is about transparency, also alpha channel is about this.
- There are different ways to get colors in CSS you can you presets like "red", you can also use rgb(255, 0, 0) for red, or hexadecimal #FF0000, or hsl (0, 100%, 50%).
	- The "hsl-model" stands for hue, saturation and lightness.
- If you want to create a shadow on an element you can use box-shadow.
- To postion two block elements like divs on the sam line set display: inline-block;
- The `rem` unit stands for root `em`, and is relative to the font size of the `html` element.

## Box-model:

In CSS, the term "box model" is used when talking about design and layout.

The CSS box model is essentially a box that wraps around every HTML element. It consists of: margins, borders, padding, and the actual content. The image below illustrates the box model:

(**src:** https://www.w3schools.com/css/css_boxmodel.asp)
![[Pasted image 20230329232103.png]] 

## Flex-container

You can set a flex-box with ´display-flex´

The flex container properties are:
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

The direct child elements of a flex container automatically becomes flexible (flex) items.

The flex item properties are:
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

You can also use media queries to create different layouts for different screen sizes and devices.

## Pseudo selectors:
These pseudo-classes relate to the location of an element within the document tree.

- :root
Represents an element that is the root of the document. In HTML this is usually the <html> element.
- :empty
- :nth-child
- :nth-last-child
- :first-child
- :last-child
- :only-child
- :nth-of-type
- :nth-last-of-type
- :first-of-type
- :last-of-type
- :only-of-type

## Display property:

/* precomposed values */
display: block;
display: inline;
display: inline-block;
display: flex;
display: inline-flex;
display: grid;
display: inline-grid;
display: flow-root;

/* box generation */
display: none;
display: contents;

/* multi-keyword syntax */
display: block flow;
display: inline flow;
display: inline flow-root;
display: block flex;
display: inline flex;
display: block grid;
display: inline grid;
display: block flow-root;

/* other values */
display: table;
display: table-row; /* all table elements have an equivalent CSS display value */
display: list-item;

/* Global values */
display: inherit;
display: initial;
display: revert;
display: revert-layer;
display: unset;

## Media query

Media query is a CSS technique introduced in CSS3.

It uses the `@media` rule to include a block of CSS properties only if a certain condition is true.

´@media only screen and (max-width: 600px) {  
body { 
background-color: lightblue;  
}
}´

## CSS Variables

You can put values in :root and reuse them later like:

:root {
  --building-color1: #aa80ff;
  --building-color2: #66cc99;
  }

color: var(--building-color2);

## CSS Grid Layout 
Grid excels at dividing a page into major regions or defining the relationship in terms of size, position, and layer, between parts of a control built from HTML primitives.

Like tables, grid layout enables an author to align elements into columns and rows. However, many more layouts are either possible or easier with CSS grid than they were with tables. For example, a grid container's child elements could position themselves so they actually overlap and layer, similar to CSS positioned elements.

.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  grid-auto-rows: minmax(100px, auto);
}

## CSS Animation

The animations CSS module lets you animate the values of CSS properties, such as background-position and transform, over time by using keyframes. Each keyframe describes how the animated element should render at a given time during the animation sequence. You can use the properties in the animations module to control the duration, number of repetitions, delayed start, and other aspects of an animation.

## CSS Transform

**CSS Transforms** is a module of CSS that defines how elements styled with CSS can be transformed in two-dimensional or three-dimensional space.

