# Intro to CSS Grid

## Learning Objectives
- Study the basic concepts of CSS Grid Layout
- Apply those concepts by building our own grids in Codepen
- Identify common pitfalls and misconceptions
- Equip ourselves with a variety of learning resources

## The Fundamentals of CSS Grid Layout
So what is CSS Grid? An actual, bona fide, honest-to-God **two-dimensional layout** in CSS. Here's what we get right out of the box:
- **P**recise item placement
- **I**mplicit grid
- **V**ertical and horizontal alignment
- **O**verlapping content
- **T**rack sizing

Yes, you read that correctly: we can now **PIVOT**.

<img src="pivot.gif"/>

> This isn't an industry-approved acronym, but it works and you may find it helpful. Also, I really wanted to use this gif, okay? Okay.

### Containers and Items and Tracks, Oh My!
We'll do some PIVOTing shortly, but first, let's establish a working vocabulary.

For starters, we have a parent element: the **GRID CONTAINER**. Any direct children of that parent element are **GRID ITEMS**. 

<img src="container-items.jpg"/>

Grid items can be placed in **GRID COLUMNS** and **GRID ROWS**. Together these columns and rows define **GRID TRACKS**.

<img src="columns-rows.jpg"/>

If this seems abstract right now, don't worry. It will get clearer as we build this stuff ourselves. Time to fire up Codepen.

### Exercise #1: Declaration of Gridependance 
🔗 Codepen: [Getting Started](https://codepen.io/solomonkane/pen/938ef9695f92af67c8284049be1410a8)

Declaring a grid is simple. Just set `display: grid` on our grid container - in this case, the div with the clever class name `.grid` - and _voila!_ We're done. Ship it.

Okay, maybe don't ship it _quite_ yet. Nothing has really changed about the appearance of our grid; the divs are still stacked on top of each other like before. Let's take a closer look... in Firefox.

> Say hello to the **Firefox Grid Inspector**. It's powerful, easy to use, and nicer than anything currently available in Chrome DevTools. It can help us visualize our layouts and prevent debugging-induced baldness. Basically, it's awesome. Read more about it [here](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts).

After popping open Firefox DevTools, we can see `display: grid` did, in fact, change _something_. There's a "grid" marker next to our `<section>`. There's also a ~~tiny waffle~~ grid icon next to our declaration in the Rules pane. Groovy. Our grid does exist after all.

Turns out, what we're missing is a column declaration. Let's add `grid-template-columns: 500px 500px` to our grid container.

That's more like it. Now we have a grid with 2 columns, each 500px wide. We can even shorten our declaration with the `repeat()` function, like so: `grid-template-columns: repeat(2, 500px)`.

We're using pixels here, but any length unit is a viable option: rems, ems, percentages, you name it. 

Grid also brings a new unit of measurement to CSS: the `fr` unit represents a fraction of the available space in a grid container, and it's here to make our grid tracks flexible AF.

> [note that fr always defers to fixed values]

We can also specify rows using `grid-template-rows`. 

_[segway into TRACK SIZING: fixed vs flexible, the `fr` unit, etc]_

_[discuss Flexbox vs CSS Grid, nature of one-dimensional layouts vs two-dimensional, including Codepen example]_

### Precise item placement
### Implicit grid
### Vertical and horizontal alignment
- justify-content, justify-items
- align-content, align-items
### Overlapping content
### Track sizing

## Let's Build a Grid... or Five
Codepen Collection: [Intro to CSS Grid](https://codepen.io/collection/5cd694b7ffc031be4a186e9fb32b97f7/)

## Gotchas to Consider
- CSS Grid is not a replacement for Flexbox

## Learning Resources
### Theory & Reference 📚
- [Get Ready for CSS Grid](https://abookapart.com/products/get-ready-for-css-grid-layout) by Rachel Andrew
- [Grid by Example](https://gridbyexample.com/)
- [CSS Grid Layout | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
### Practice 💪
- [CSS Grid Garden](http://cssgridgarden.com/)
- Jonas Schmedtmann's [Advanced CSS and Sass](https://redventures.udemy.com/advanced-css-and-sass/learn/v4/overview)
- Wes Bos' [CSS Grid Course](https://cssgrid.io/)
### Tools 🛠
- [Firefox Grid Inspector](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts)
