# Recreating a Landing Page

To practice CSS I decided to recreate this page: [React Training](https://reacttraining.com/courses//images/blue-fade.svg).

It is a rather simple page but it is also elegant and responsive in a way that I would like to practice
and study on a deeper level.

I imagine these React guys know what they are doing.

It contains some common parts of a web page:
+ a hamburger menu
+ header area with logo icon, text logo and subheading
+ Call to Action button
+ Tagline
+ Footer
+ Extra footer with copywrite info

It is organized in blocks or cards which is good for responsiveness, since they can be moved around
without messing up the page.

It also uses some subtle background forms for decoration.

So, let's get started. Mobile first, so I shrink it in a separate window of Chrome and then work from Codepen
in another window. I will first create the page without looking at the source code, since that feels like cheating.

Usually I use SCSS but this page is so simple. I also would like to try out using native CSS variables. Why cross the river to fetch water, as they say in Sweden.

I plan to recreate the page first, then tweak it a little. Or a lot, we will see.

- - -

## Semantic HTML

After marking up everything in semantic HTML ( nav, header, footer, main, some sections and articles, I am ready to start.
First thing that would be nice is some color. The background is also logical to start with, since I am thinking in terms of layers, and this is the layer that is at the back of everything.

## Colorzilla

Using a chrome extension, [ColorZilla](https://www.colorzilla.com/), which is a color picker and eye dropper tool, I can easily get the hex code for the background.

![Colorzilla Chrome Extension](/colorzilla.png)

## CSS Variables

Since CSS Custom Properties (variables) inherit the value from its parent, I set my background-color to the :root pseudo class. This class is identical to the html element but its specificity is higher and therefore good for when setting CSS variables.

```css
:root {
  --background: #EDF4F7;
}

body {
  background-color: var(--background);  
}
```
This way I can use the variables in any (child) element in the document.

## CSS or SVG?

The background decorations, a couple of simple forms, can be made in a illustrator program as a PNG or SVG. Looking at the shapes, they are very simple, and could be easily made also with the help of some CSS. Lately I have been learning CSS Illustration which has made me more confident with this technique. I am going for CSS.

There are three rectangles with rounded edges, or perhaps triangles are a better fit, since the sides are not 90 degrees but larger. This is what it looks like:



