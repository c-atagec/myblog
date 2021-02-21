+++
title = "What is CSS display property?"
date = "2021-02-21"
description = "Article that explains CSS display property and it's display: block, display: inline-block, display: inline, display: none values"
tags = [
  "CSS",
  "display",
  "none",
  "block",
  "inline-block",
  "inline"
]

+++

### "There are 8 ways to do same thing"

### "Should I use inline or block?"

We all have those questions popping in our heads when we try to build something new with HTML / CSS, how the elements is going to be displayed? on top of each other? side by side?

It can be frustrating when you're just starting out to learn HTML / CSS.

So, let's take a look at what **display** property is?

- `display` property tells browser how elements is going to be rendered by the browser.

There are plenty of display values, we're going to focus on just 4 of them. Those are:

- `display: block;`
- `display: inline;`
- `display: inline-block;`
- `display: none;`


## display: block;

- Block level elements starts at a **new line** and the elements comes after it **has to stars at a new line.**
- It takes up the **largest** available space.
- We **can** control width and height of the element.
- Elements are affected by margin and padding.
- Block level elements stacks **on top of each other** ( like hamburger ingredients ).
- Here are the list 
{{< highlight html >}}
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Hamburger</title>
    </head>
    <body>
      <div class="bun">Bun</div>
      <div class="cucumber">Cucumber</div>
      <div class="onion">Onion</div>
      <div class="bacon">Bacon</div>
      <div class="cheese">Cheese</div>
      <div class="meat">Meat</div>
      <div class="tomato">Tomato</div>
      <div class="lettuce">Lettuce</div>
      <div class="bun">Bun</div>
    </body>
    </html>
{{< /highlight >}}

- As we can see, block level elements creates a new line for preceding and subsequent elements ( **div** is a block level element )
![Hamburger](/images/hamburger-graph.jpg)

## display: inline;

- Inline elements take **minimum** available content space
- We **can't** control width and height of the element,
- Only **horizontal dimensions** are affected by margin and padding properties, element's width and height will remain **unaffected.**
{{< highlight html >}}
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Train</title>
    </head>
    <body>
      <span class="locomotive">Locomotive</span>
      <span class="wagon-1">Wagon 1</span>
      <span class="wagon-2">Wagon 2</span>
      <span class="wagon-3">Wagon 3</span>
      <span class="wagon-4">Wagon 4</span>
      <span class="wagon-5">Wagon 5</span>
    </body>
    </html>
{{< /highlight >}}
- Inline elements are like train wagons. They stand **side by side**.
![Train](/images/train-span.jpg)

## display: inline-block;

- Combination of block and inline level elements
- Main difference from inline level elements is we **can** control the width and height properties.
- Elements are affected by margin and padding properties ( both vertically and horizontally )

## display: none;

- It simply **hides** the element.
- Element that has `display: none;` property is not going show up at screen and other elements will act like it doesn't exist.

## Let's see how they interact with each other
We learned what `inline`, `inline-block`, `block` level elements are. Let's take a look at how they interact with each other.

Imagine you purchased *two paintings* , *a TV* and a *TV unit* to decorate your favorite corner of the house.

First, you put the *TV* between the *paintings* vertically and the *TV* unit at the bottom. (`display: block;` on each element)
![display-block-each](/images/display-1.png)

Hmmm, that looks *'fine'* but I think we can do better. Let's put paintings and the TV side by side (`display: inline;`)
![display-inline](/images/display-2.jpg)

That didn't work either. ( Remember, we **cannot** control width and height properties of inline level elements ) I think we can make it work by giving each of them the space they need. ( `display: inline-block;` )
![display-inline-block](/images/display-3.jpg)


Yay, it worked. As a finishing touch, let's center that *TV unit* at the bottom ( display inline-block at TV unit element ).
![display-block-tv-unit](/images/display-4.jpg)


So, you might ask how that *TV unit* is centered automatically. Because, we gave `text-align: center;` property to their parent element.

You can take a further look at this [codepen,](https://codepen.io/hunata/full/JjbJLJw "Example Codepen")to view source code click **Change View** at the **top right corner**, and select **Editor View**.

Here are some of the helpful links: 
- [Block Level Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements "Block Level Elements")
- [Inline Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements "Inline Elements")
- [Block and Inline Layout in Normal Flow](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flow_Layout/Block_and_Inline_Layout_in_Normal_Flow "Block and Inline Layout in Normal Flow")


