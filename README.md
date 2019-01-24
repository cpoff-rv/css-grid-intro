# Intro to CSS Grid

## Contents
- [Learning Objectives](#learning-objectives)
- [The Fundamentals of CSS Grid](#the-fundamentals-of-css-grid-layout)
- [Precise Item Placement](#the-p-precise-item-placement)
- [Implicit Grid](#the-i-implicit-grid)
- [Vertical and Horizontal Alignment](#the-v-vertical-and-horizontal-alignment)
- [Overlapping Content](#the-o-overlapping-content)
- [Track Sizing](#the-t-track-sizing)
- [Extra, Extra](#extra-extra)
- [Learning Resources](#learning-resources)

## Learning Objectives
- Study the basic concepts of CSS Grid Layout
- Apply those concepts to our own grids in Codepen
- Identify common pitfalls and misconceptions
- Equip ourselves with a variety of learning resources

Full Codepen collection [here](https://codepen.io/collection/XEmBgV/2/).

## The Fundamentals of CSS Grid Layout
So what is CSS Grid? An actual, bona fide, honest-to-God **two-dimensional layout** in CSS. Here's what we get right out of the box:
- **P**recise item placement
- **I**mplicit grid
- **V**ertical and horizontal alignment
- **O**verlapping content
- **T**rack sizing

Yes, you read that correctly: we can now **PIVOT**.

<img src="pivot.gif"/>

> This isn't an industry-approved acronym, but it works and you may find it helpful. Also, I really wanted to use this gif.

### Containers and Items and Tracks, Oh My!
We'll do some PIVOTing shortly, but first, let's establish a working vocabulary.

For starters, we have a parent element: the **GRID CONTAINER**. Any direct children of that parent element are **GRID ITEMS**.

Grid items can be placed in **GRID COLUMNS** and **GRID ROWS**. Together these columns and rows define **GRID TRACKS**.

Everything about Grid is easier to understand when you can actually _look_ at it, so let's fire up Codepen... in Firefox.

> Say hello to the **Firefox Grid Inspector**. It's powerful, easy to use, and nicer than anything currently available in Chrome DevTools. It can help us visualize our layouts and prevent debugging-induced baldness. Basically, it's awesome. Read more about it [here](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts).

### Example #1: Declaration of Gridependance 
🔗 **Codepen**: [Basic Grid](https://codepen.io/solomonkane/pen/938ef9695f92af67c8284049be1410a8)

Declaring a grid is simple. All we have to do is set `display: grid;` on our grid container and _voila!_ We're done. Ship it.

Okay, maybe don't ship it _quite_ yet. Nothing has really changed about the appearance of our grid; the items are still stacked like before. Let's take a closer look...

After popping it open Firefox DevTools, we can see `display: grid` did, in fact, change something. There's a "grid" marker next to our `<section>`. There's also a ~~tiny waffle~~ grid icon next to our declaration in the Rules pane. Groovy. Our grid does exist after all.

Turns out, what we're missing is a column declaration. Let's add `grid-template-columns: 500px 500px;` to our grid container.

That's more like it. Now we have a grid with 2 columns, each 500px wide. We can even shorten our declaration with the `repeat()` function: `grid-template-columns: repeat(2, 500px);`.

We're using pixels here, but any length unit is a viable option: rems, ems, percentages, you name it. We can even mix these units: `grid-template-columns: 33% 100px 20vw 15rem 4em;` is a perfectly valid rule, believe it or not.

Grid also brings a new unit of measurement to CSS: the `fr` unit represents a fraction of the available space in a grid container, and it's here to make our grid tracks flexible AF. 

Why should we use `fr` over other flexible units, like percentages? Long story short, [it saves us a bunch of math](https://css-tricks.com/introduction-fr-css-unit/).

Let's use `fr` on our grid: `grid-template-columns: repeat(2, 1fr);`.

> ⚠️ `fr` will always defer to other units. If we write `grid-template-columns: 250px 1fr;`, 250px will be taken out of the available space, and the remaining space goes to the `fr`. In other words, if other units are eating the grid, `fr` only gets the leftovers.

Columns are great, but isn't Grid all about two-dimensional control? Where are our rows? 

We can define rows using - you guessed it - `grid-template-rows`. As with columns, any length unit (or combination of length units) is fair game.

Last but not least, we can define gutters with `grid-row-gap` and `grid-column-gap`... or with the shorthand `grid-gap`.

Let's PIVOT.

## The P: Precise Item Placement
Grid allows us to get super specific about where items live in our grid container. We can do this with line numbers, named lines, and grid areas. Suddenly projects like this [masonry effect](https://codepen.io/solomonkane/pen/VEyWQO) can be done with relative ease.

### Example #2: "The numbers, Mason! What do they mean?"
🔗 **Codepen**: [Precise Item Placement](https://codepen.io/solomonkane/pen/7a803d257a3a65e479bf824a70a1095c)

Using `grid-column-start` and `grid-column-end`, we can tell grid items which column to occupy. If we want to specify the row, we can use `grid-row-start` and `grid-row-end`.

> ⚠️ These properties can be shortened to `grid-column` and `grid-row`, respectively. Just provide the starting and ending values, seperated by a slash: `grid-column: 1 / 2`.

Let's use this feature to move `grid__item--rogue` around our grid.

### Using line numbers

```css
.grid__item--rogue {
  grid-column: 2 / 3;
  grid-row: 3 / 4;
}
```

This puts our grid item in the _column_ between (vertical) lines 2 and 3, and in the _row_ between (horizontal) lines 3 and 4. 

> If you want to visualize the lines, you can toggle `Display line numbers` in Firefox Grid Inspector.

Alternatively, we could write `grid-column: 2 / span 1;` to make our item span a single column. 

We could also write `grid-column: 2 / -1;`, which just means "start at line 2 and continue until the end of the track".

### Using named lines

```css
.grid {
  grid-template-columns: [col-1-start] 1fr [col-1-end col-2-start] 1fr [col-2-end];
  grid-template-rows: [row-1-start] 100px [row-1-end row-2-start] 100px [row-2-end row-3-start] 100px [row-3-end];
}

.grid__item--rogue {
  grid-column: col-1 / col-2;
  grid-row: row-2;
}
```

Named lines are just syntactic sugar. At the end of the day, you're still working with lines, but names are more meaningful than numbers.

> You can name the lines anything you want, provided you still use `-start` and `-end`. 

### Using grid areas
Say hello to `grid-template-areas`. This property allows us define grid areas with ascii art: each word represents a column and each line represents a row.

```css
.grid {
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas:
          "left right"
          "left right"
          "bottom bottom";
}

.grid__item--rogue {
  grid-area: bottom;
}
```

This takes some getting used to, but once you wrap your head around it, it's fantastic - particularly when you apply it to something like [site layout](https://codepen.io/solomonkane/pen/3c7e2d965ff468fbe294b585142aa379?editors=1100).

## The I: Implicit Grid
Manual positioning for grid items is cool, but that's not always what you want. Most of the time you'll have a few grid items you want to position, while everything else should just... fall into place. No extra math, please.

Welcome to the implicit grid.

As a matter of fact, we've already seen this in action. In our first grid example, we didn't explictly position _any_ of the grid items, and they still "magically" arranged themselves into columns and rows. 

This is possible thanks to CSS Grid's auto-placement rules. When we create a grid, each grid item is automatically placed in a cell on the first row. When there's no more room on the explicit row, a new _implicit_ row is automatically created for us. 

### Example #3: Auto-Placements Assemble
🔗 **Codepen**: [Explicit Grid vs Implicit Grid](https://codepen.io/solomonkane/pen/ad1ce44713514030614a96eebef6c7bc)

We have six items laid out on a 3x2 grid. This grid is _explicitly_ defined, and all our grid items fit snugly in it. But what if we add a seventh item?

Because there's no more room in the explicit grid, a brand new row is created for us. This row is part of the implicit grid, _because we didn't define it._ Grid defined it for us.

> ⚠️ If you're unsure where the explicit grid ends and the implicit grid begins, Firefox Grid Inspector can distinguish between the two. The solid black line around your items is the explicit grid. The tiny dotted line that picks up right after it is the implicit grid.  

As you've probably noticed, the new implicit row is sized differently. By default, all rows on the implicit grid are auto-sized: they're only big enough to house the content inside of them without overflowing. We can change that with `grid-auto-rows` property. By adding `grid-auto-rows: 100px;` to our grid container, we make _all_ rows - explicit and implicit - the same size.

By default Grid arranges by row - just like Flexbox. And just like Flexbox, we can change that: only instead of using `flex-direction`, we use the `grid-auto-flow` property. With `grid-auto-flow: column;`, any items that can't fit on the explicit grid will be placed in new implicit _columns_.

> If you're trying to size implicit columns, `grid-auto-columns` is the ~droid~ property you're looking for.

## The V: Vertical and Horizontal Alignment
One mistaken assumption about Grid is that it replaces Flexbox. It doesn't. You can and should use both, because they're meant to solve different problems. Flexbox is intended for one-dimensional layouts - you want horizontal **or** vertical control. Grid is intended for two-dimensional layouts - you want horizontal **and** vertical control.

<img src="why-not-both.gif"/>

To better understand this difference, let's look at an example.

> Rachel Andrew and MDN both use variations of this example, so you know it's a good one.

### Example #4: "You are traveling through another dimension..."
🔗 **Codepen**: [One-Dimensional vs Two-Dimensional](https://codepen.io/solomonkane/pen/96ea9b0fd828481a8fa04c0f13fd16f8)

On the left we have a "grid" with Flexbox. We've got `flex-wrap: wrap;` on the parent, and `flex: 1 1 calc(50% - 4px);` on the children. Each child can grow and shrink, with a `flex-basis` equal to half the size of the container (minus the 2px border on each side). Our "grid" looks pretty decent until we get to the fifth item. Instead of lining up under other the items, the fifth item has grown to fill the entire container. 

This isn't a bug. It's how Flexbox works. When our flex item wraps, it creates a new row, and that new row becomes a new flex container. 

In some cases, this may be exactly what you want. In other cases, not so much. That's where Grid steps in.

On the right of our example, we have a grid with CSS Grid. Using just one property - `grid-template-columns: repeat(2, 1fr);` - we already have a truer grid than we had with Flexbox. The fifth item is properly lined up under the other items.

A good question to ask when deciding whether to use Flexbox or Grid is: "Do I need vertical and horizontal alignment in my layout?" If you need both, Grid is your best bet. If you only need one or the other, Flexbox will do just fine.

Another question to ask is: "Content _out_ or layout _in_?" If you want items laid out without necessarily lining up, and you want _content_ to dictate the size of each item... Flexbox is perfect for that.

But Grid works from the _layout_ in. You create a grid, then place items in it (or let auto-placement do that for you). Either way, the items are laid out according to the grid, not according to their content. 

As a rule of thumb, if you find yourself trying to make Flexbox less flexible, use Grid. And if you find yourself trying to make Grid _too_ flexible, use Flexbox.

## The O: Overlapping Content
Laying items out side by side is great, but it's also possible for more than one grid item to occupy the same column (or row).

### Example #5: "The name is Lap. Over Lap."
🔗 **Codepen**: [Overlapping Items](https://codepen.io/solomonkane/pen/1521429fcd1d96d4d1fac4b433e100ab)

Our grid is divided into 3 columns, with implicit rows being created as needed. 

Let's make `.grid__item--1` stretch across the entire first row:

```css
.grid__item--1 {
  grid-column: 1 / -1;
  grid-row: 1;
}
```

Notice how our grid automatically shifts the other items around to accomodate our precise placement of `.grid__item--1`. Item 2 is moved over and down. We want Item 2 to stay put, so let's manually place it: 

```css
.grid__item--2 {
  grid-column: 3 / -1;
}
```

This put Item 2 in the correct column, but it's still underneath Item 1 because we haven't specified the row. Without explict instruction, Grid will make assumptions about the placement and just bump it down.

```css
.grid__item--2 {
  grid-column: 3 / -1;
  grid-row: 1;
}
```

If we wanted to put Item 1 on top of Item 2, we can use `z-index`.

A practical upside to this feature is that it may replace the need for `position: absolute;` in certain cases.


## The T: Track Sizing
We've seen some of this already, but to recap: Grid offers us the ability to create fixed _and_ flexible track sizes using various length units. We can use pixels if we want a fixed grid, or the new `fr` unit if we want a flexible grid.

- Fixed 3-column grid: `grid-template-columns: 250px 250px 250px;`

- Flexible 3-column grid: `grid-template-columns: 1fr 1fr 1fr;`

We also learned that we can mix units: `grid-template-columns: 33% 100px 20vw 15rem 4em` is crazy-looking, but valid.

We used the `repeat()` function, but there's another function worth mentioning: the almighty `minmax()`. 

### Example #5: Minimum Maximum Overdrive
🔗 **Codepen**: [The Almighty Minmax()](https://codepen.io/solomonkane/pen/20b7f47c2927c4eb4e948b9125bca56b?editors=1100)

`minmax()` takes two parameters: a `min` and a `max` (didn't see that coming, did you?). Using these parameters, `minmax()` ( per MDN) "defines a size range greater than or equal to min and less than or equal to max." So it's basically just `min-width` and `max-width` coming together to form a super CSS robot.

<img src="voltron.gif"/>

A few examples:
- `grid-template-columns: 700px minmax(300px, 500px);`: The first column will be fixed at 700px. The second column will be flexible, with a minimum width of 300px and a maximum width of 500px.

- `grid-template-columns: 700px minmax(300px, 1fr);`: The first column will be fixed at 700px. The second column will have a flexible width, growing as much as the remaining space allows, but not shrinking below 300px.

- `grid-template-columns: 700px minmax(min-content, 500px);`: The first column will be fixed at 700px. The second column will have a minimum width of the content's _smallest_ possible size (in this case, the text will wrap). The second column will not grow larger than 500px.

- `grid-template-columns: 700px minmax(max-content, 500px);`: The first column will be fixed at 700px. The second column will have a minimum width of its content's _largest_ possible size (in this case, the text will _not_ wrap). The second column will not grow larger than 500px.

But wait. There's more.

### Auto-Fill vs Auto-Fit
So far, we've been hardcoding the number of columns we want, with rules like `repeat(2, 500px)`. But what if we don't know - or care - how many columns our grid should have? Wouldn't it be nice if Grid just figured that out for us?

Ask and ye shall recieve. Let's talk about two new keywords: `auto-fill` and `auto-fit`. 

- `auto-fill`: Fills the row with as many columns as possible. It creates a new implicit column whenever there's room for  one. Even if the new implicit column is empty, it will still take up space on the grid.

- `auto-fit`: Fits the current columns (i.e. columns with actual content) into the row by expanding them. It also creates new implicit columns, but collapses any empty columns to 0px width.

So both of these keywords create new columns, but `auto-fit` collapses columns _without_ content to 0px width, then expands the columns _with_ content to take up all that extra space. The real difference, then, is how these keywords treat empty columns. 

We can see the difference in [this example](https://codepen.io/solomonkane/pen/7c4d2058024fca571761278ca2159efd). Right now, that difference is only visible if we open Grid Inspector. Throw `fr` into the mix, and suddenly it's much more obvious.

```css
.grid--fill {
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
}

.grid--fit {
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
}
```

Don't get too bogged down with it, though. For our purposes we're only going to use `auto-fit`, and the benefit of it will be a bit clearer.

### Responsive CSSorcery
What if I told you that by combining `repeat()` with `minmax()` and `auto-fit`, we can get a fully responsive grid without writing a single media query?

<img src="sorcery.gif"/>

We caught a glimpse of this with our last example, but for clarity's sake, let's go back to [the minmax() Codepen](https://codepen.io/solomonkane/pen/20b7f47c2927c4eb4e948b9125bca56b) and give our grid container the following declaration:

```css
.grid {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

Our grid is now made up of 5 columns, each with a minimum width of `200px` and a maximum width of `1fr`, which means it will grow beyond 200px but won't shrink below 200px. By pairing `auto-fit` with `minmax()`, we're telling Grid to figure out the number of columns we need _and_ make sure those columns expand across any available space. Now watch what happens when we resize the window.

_Magic._

We went from 5 columns... to 4 columns... to 3 columns... to 2 columns... to 1 column. No media queries required.

Does this mean we can scrap media queries entirely? _Negative, Ghost Rider._ It just means we have a new tool in our toolbox. We'll still use media queries, but we won't have to use them as often or in the same places. Responsive design just got even easier.

## Extra, Extra
You probably have questions, so here are some answers...

### "But what about browser support?"
The elephant in the room, but it really shouldn't be. We're currently at [**85.6%** unprefixed usage](https://caniuse.com/#feat=css-grid), which is significant. That number goes up if you're willing to polyfill or provide a fallback layout for spoil-sports like IE11. Progressive enhancement, folks. 

If you're still not convinced, consider Rachel Andrew's take [here](https://rachelandrew.co.uk/archives/2017/07/04/is-it-really-safe-to-start-using-css-grid-layout/). The article is from July 2017, so the support stats are dated. The principles, however, are rock solid.

### "And accessibility? What about that?" 
Check out [this helpful overview](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_Layout_and_Accessibility) from MDN, with further reading included.

### "Didn't I hear something about subgrid?"
Probably. [CSS Grid Level 2 is coming](https://www.smashingmagazine.com/2018/07/css-grid-2/). Yet another reason to start learning, y'know, Level 1.


## Learning Resources
### Theory & Reference 📚
- Rachel Andrew's [Get Ready for CSS Grid](https://abookapart.com/products/get-ready-for-css-grid-layout)
- [Grid by Example](https://gridbyexample.com/)
- [CSS Grid Layout | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)

### Practice 💪
- [CSS Grid Garden](http://cssgridgarden.com/)
- Jonas Schmedtmann's [Advanced CSS and Sass](https://redventures.udemy.com/advanced-css-and-sass/learn/v4/overview)
- Wes Bos' [CSS Grid Course](https://cssgrid.io/)

### Tools 🛠
- [Firefox Grid Inspector](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts)
