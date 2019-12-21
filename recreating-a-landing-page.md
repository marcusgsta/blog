# Recreating a Landing Page

To practice CSS I decided to recreate this page: [React Training](https://reacttraining.com/courses//images/blue-fade.svg).

It is a rather simple page but it is also elegant and responsive in a way that I would like to practice
and study on a deeper level.

I divided my work into these parts:
+ Responsive menu
+ Background shapes and logo remade in CSS
+ Consistent margins
+ Card
+ CSS variables
+ Footer
+ Conclusion

The webpage is organized in blocks or cards which is good for responsiveness, since they can be moved around
without messing up the page.

It also uses some subtle background forms for decoration.

So, let's get started. Mobile first, so I shrink it in a separate window of Chrome. I will first create the page without looking at the source code, since I am doing this to learn and to figure out solutions for myself.

I use SASS but I would like to try out using native CSS custom properties.

- - -

## Semantic HTML

After marking up everything in semantic HTML ( nav, header, footer, main, some sections and articles, I am ready to start.
First thing that would be nice is some color. The background is also logical to start with, since I am thinking in terms of layers, and this is the layer that is at the back of everything.

## Colorzilla

Using a chrome extension, [ColorZilla](https://www.colorzilla.com/), which is a color picker and eye dropper tool, I can get the color code for the background.

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

The background decorations, a couple of simple forms, can be made in a illustrator program as a PNG or SVG. Looking at the shapes, they are very simple, and could also be made with the help of some CSS. It's time to put my CSS knowledge to use.

There are three rectangles with rounded edges, or perhaps triangles are a better fit, since the sides are not 90 degrees but larger. This is what it looks like:

![Background Shapes](background-shapes.png)

I create a new div with the class of 'shapes'. Since each div can have two extra div:s in the form of the pseudo-elements :before and :after, this is all that is needed for the HTML.

The CSS triangle which uses borders and zero width and height creates a right-angled triangle, but I need a wider angle. I also try using clip-path but this doesn't seem to let me do rounded angles.


## SASS Mixins to not repeat code

Since I will be repeating the same shape three times, I could also make use of a SASS mixin, to reduce repeating lines of code. However, this seems not to be available using CSS only. Since I'd like to delve into writing functions I change my mind from previously and add SCSS preprocessor to my Codepen.

## SVG

The shapes are overlapping and using opacity, which makes it even more tricky using only CSS. Finally I go for SVG, using an [editor](https://svg-edit.github.io/svgedit/releases/svg-edit-2.8.1/svg-editor.html) to draw it and then copy the SVG code.

It took some time adjusting the SVG shapes with CSS, this is what I got:

![My background shapes in SVG](my-shapes.png)

SVG is actually excellent since it is possible to change and tweak the color and size of them with CSS. I used hsla-color, so that I could easily adjust the intensity, lightness and opacity.

The HTML:

```html
<svg viewbox="-100 0 500 600" id="shapes" xmlns="http://www.w3.org/2000/svg">
 <g>
  <path id="shape-one" d="m0.565004,-0.718787l85.767768,69.509633c0,0 20.584272,20.138305 61.752803,1.299244c41.16853,-18.839061 165.531798,-79.253964 165.531784,-79.253975" stroke-width="0"/>
   <path id="shape-two" d="m0.565004,-0.718787l85.767768,69.509633c0,0 20.584272,20.138305 61.752803,1.299244c41.16853,-18.839061 165.531798,-79.253964 165.531784,-79.253975" stroke-width="0"/>
   <path id="shape-three" d="m0.565004,-0.718787l85.767768,69.509633c0,0 20.584272,20.138305 61.752803,1.299244c41.16853,-18.839061 165.531798,-79.253964 165.531784,-79.253975" stroke-width="0"/>
 </g>
</svg>
```
The CSS, using SCSS @mixin and @include to reduce amount of code.:

```css
:root {
  --background: #EDF4F7;
  --bg-shape1: hsla(196, 85%, 86%, 0.4);
  --bg-shape2: hsla(196, 95%, 84%, 0.4);
  --bg-shape3: hsla(196, 95%, 84%, 0.4);
}
* {
  padding: 0;
  margin: 0;
}
body {
  background-color: var(--background);
  z-index:-10;
}

@mixin style-shape($top, $left, $color, $width,$height) {
  position: absolute;
  top: $top;
  left:$left;
  width:$width;
  height:$height;
  fill:$color;
  z-index:-5;
}

#shapes {
  width:100%;
  height:100%;
}
#shape-one {
  @include style-shape(30px, 0, var(--bg-shape1), 100%, 100%);
  transform:translateY(2px) translateX(-30px) scale(1.2);
}
#shape-two {
   @include style-shape(-50px, 0, var(--bg-shape2), 100%, 100%);
  transform:translateY(-20px)
            translateX(20px);
}
#shape-three {
   @include style-shape(-50px, 0, var(--bg-shape3), 100%, 100%);
  transform:translateY(-10px)
            translateX(150px)
            scale(1.3);
}
```
I also made it reponsive, setting the sizes to percentages.

## The Menu
Starting with the menu button for a mobile sized screen, I put some color, border-radius and a little triangle icon that was part of the font. It also needed some JavaScript. The tricky part was to hide the menu when clicking outside of the menu.

```javascript
let menuBtn = document.querySelector('#menu-btn');
let menuOptions = document.querySelector('#menu-options');
let workShopLink = document.getElementById('workshop');


function closeMenu(e) {    
 if (e.target != menuBtn && menuOptions.style.display === "block") {
  menuOptions.style.display = "none";   
 }
 document.body.removeEventListener("click", closeMenu);
}

function showHideMenu(e) {
    console.log(menuOptions.style.display)
    if ( menuOptions.style.display === "none" || menuOptions.style.display === "") {
        menuOptions.style.display = "block";
        menuOptions.style.outline = "none";
        workShopLink.focus();
        // add click event for body
        document.body.addEventListener("click", closeMenu, true);
    } else {
        menuOptions.style.display = "none";
    }
}

menuBtn.addEventListener("click", showHideMenu);


workShopLink.addEventListener("mouseover", function() {
  workShopLink.blur();
});

function focusMenu() {
    menuOptions.classList.add("focused");
}
function blurMenu() {
    menuOptions.classList.remove("focused");
}

menuOptions.addEventListener("mouseover", blurMenu);
menuOptions.addEventListener("mouseout", focusMenu);

```
I also added some extra functionality: an outline on the menu items when the mouse leaves them. This was fixed with the events mouseover and mouseout. Finally I got a menu that could be shown and hidden on mouseclick.

## Box Shadow for :focus

This handy trick I got from Kevin Powell [Box Shadow for :focus](https://youtu.be/Mvu5OMGcdVA), that made it possible to get rounded edges, something that is not possible with the outline property.
```
  .focused {
      box-shadow:0 0 0 2px var(--background),
                 0 0 0 5px dodgerblue;
  }
```
The .focused class is added on the mouseout event. It utilizes 2 box shadows on top of each other to get a nice rounded frame.


## Main Layout

I like to use Flexbox since it was made for responsive sites. I set the main-element to display:flex.



## Conclusion

+ CSS vs SCSS variables
I like that I can use CSS custom properties and exclude the need for a CSS preprocessor like SASS. The thing I miss with SASS variables is the functions I can use with them, like lighten(), darken() which can be really handy for tweaking color schemes right in the editor. The thing is that even if I have SASS installed, I can't tweak the CSS custom properties. If I want that functionality I need to write SASS variables ($red).
