# Intro to CSS Grid

[ask if anyone has used it]
[we're using it on ADT.com]
[masonry, how would you do it before CSS Grid]
[some of these topics will overlap with each other, but that's okay, the goal is to be an introducton and get people excited about Grid]_

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

## Precise Item Placement
Grid allows us to get super specific about where items live in our grid container. We can do this with line numbers, named lines, and grid areas. Suddenly projects like this [masonry effect](https://codepen.io/solomonkane/pen/VEyWQO) can be done with relative ease.

### Example #2: Numbers, Names, Areas
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
  grid-template-rows: [row-1-start] 1fr [row-1-end row-2-start] 1fr [row-2-end row-3-start] 1fr [row-3-end];
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
  grid-template-rows: 1fr 1fr 1fr;
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

## Implicit Grid
Manual positioning for grid items is cool, but that's not always what you want. Most of the time you'll have a few grid items you want to position, while everything else should just... fall into place. No extra math, please.

Welcome to the implicit grid.

As a matter of fact, we've already seen this in action. In our first grid example, we didn't explictly position _any_ of the grid items, and they still "magically" arranged themselves into columns and rows. 

This is possible thanks to CSS Grid's auto-placement rules. When we create a grid, each grid item is automatically placed in a cell on the first row. When there's no more room on the explicit row, a new _implicit_ row is automatically created for us. 

### Example #3: Explicit vs Implicit
🔗 Codepen: [Explicit Grid vs Implicit Grid](https://codepen.io/solomonkane/pen/ad1ce44713514030614a96eebef6c7bc)

We have six items laid out on a 3x2 grid. This grid is _explicitly_ defined, and all our grid items fit snugly in it. But what if we add a seventh item?

Because there's no more room in the explicit grid, a brand new row is created for us. This row is part of the implicit grid, _because we didn't define it._ Grid defined it for us.

> ⚠️ If you're unsure where the explicit grid ends and the implicit grid begins, Firefox Grid Inspector can distinguish between the two. The solid black line around your items is the explicit grid. The tiny dotted line that picks up right after it is the implicit grid.  

As you've probably noticed, the new implicit row is sized differently. By default, all rows on the implicit grid are auto-sized: they're only big enough to house the content inside of them without overflowing. We can change that with `grid-auto-rows` property. By adding `grid-auto-rows: 100px;` to our grid container, we make _all_ rows - explicit and implicit - the same size.

By default Grid arranges by row - just like Flexbox. And just like Flexbox, we can change that: only instead of using `flex-direction`, we use the `grid-auto-flow` property. With `grid-auto-flow: column;`, any items that can't fit on the explicit grid will be placed in new implicit _columns_.

> If you're trying to size implicit columns, `grid-auto-columns` is the ~droid~ property you're looking for.

## Vertical and Horizontal Alignment
One mistaken assumption about Grid is that it replaces Flexbox. It doesn't. You can and should use both, because they're meant to solve different problems. Flexbox is intended for one-dimensional layouts - you want horizontal **or** vertical control. Grid is intended for two-dimensional layouts - you want horizontal **and** vertical control.

To better understand this difference, let's look at an example.

> Rachel Andrew and MDN both use variations of this example, so you know it's a good one.

### Example #4: "You are traveling through another dimension..."
🔗 Codepen: [One-Dimensional vs Two-Dimensional](https://codepen.io/solomonkane/pen/96ea9b0fd828481a8fa04c0f13fd16f8)

On the left we have a "grid" with Flexbox. We've got `flex-wrap: wrap;` on the parent, and `flex: 1 1 calc(50% - 4px);` on the children. Each child can grow and shrink, with a `flex-basis` equal to half the size of the container (minus the 2px border on each side). Our "grid" looks pretty decent until we get to the fifth item. Instead of lining up under other the items, the fifth item has grown to fill the entire container. 

This isn't a bug. It's how Flexbox works. When our flex item wraps, it creates a new row, and that new row becomes a new flex container. 

In some cases, this may be exactly what you want. In other cases, not so much. That's where Grid steps in.

On the right of our example, we have a grid with CSS Grid. Using just one property - `grid-template-columns: repeat(2, 1fr);` - we already have a truer grid than we had with Flexbox. The fifth item is properly lined up under the other items.

A good question to ask when deciding whether to use Flexbox or Grid is: "Do I need vertical and horizontal alignment in my layout?" If you need both, Grid is your best bet. If you only need one or the other, Flexbox will do just fine.

Another question to ask is: "Content _out_ or layout _in_?" If you want items laid out without necessarily lining up, and you want _content_ to dictate the size of each item... Flexbox is perfect for that.

But Grid works from the _layout_ in. You create a grid, then place items in it (or let auto-placement do that for you). Either way, the items are laid out according to the grid, not according to their content. 

As a rule of thumb, if you find yourself trying to make Flexbox less flexible, use Grid. And if you find yourself trying to make Grid _too_ flexible, use Flexbox.

## Overlapping Content
Laying items out side by side is great, but it's also possible for more than one grid item to occupy the same grid cell. I haven't used this feature much yet, but it's handy to know.

### Example #5: Overlap
🔗 Codepen: [Overlapping Items](https://codepen.io/solomonkane/pen/1521429fcd1d96d4d1fac4b433e100ab)

We have a grid divided into 3 columns, and we're letting auto-placement make the implicit rows for us, as needed. 

Let's make `.grid__item--1` stretch full width on the first row: 
```
grid-column: 1 / -1;
grid-row: 1;
```

Notice how the grid automatically shifts the other items around to accomodate our explicit placement of `.grid__item--1`. Item 2 just shifted over and down. But we want Item 2 to stay put. So let's explicitly place it: `grid-column: 3 / -1`.

Item 2 still isn't occupying the same cell. That's because we haven't specified the row. Without explictly telling it which row to occupy, Grid will make assumptions about the placement and just bump it down. Let's specify row: `grid-row: 1`.

Long story short: the more explicit you get with item placement, the higher priority it will be given.

If needed, we can use `z-index` to place Item 1 on top of Item 2.

I haven't encountered a ton of use cases for overlapping, but one potential plus is that it may replace the need for `position: absolute` in certain cases.

## Track Sizing
We looked at this earlier, but to recap: Grid offers us the ability to create fixed _and_ flexible track sizes. Whether you're making rows or columns, you can use any length unit to make your grid. Pixels if you want a fixed grid, percentages or the new `fr` unit if you want a flexible grid.

- Fixed 5 column grid: `grid-template-columns: 250px 250px 250px 250px`

- Flexible 5 column grid: `grid-template-columns: 1fr 1fr 1fr 1fr 1fr`

We also saw that we can mix units: `grid-template-columns: 33% 100px 20vw 15rem 4em` is valid.

We looked at the `repeat()` function, but there's another function worth mentioning: the almighty `minmax()`. 

### Example #5: Minmax()
🔗 Codepen: [The Almighty Minmax()](https://codepen.io/solomonkane/pen/20b7f47c2927c4eb4e948b9125bca56b?editors=1100)

`minmax()` takes two parameters: a `min` and a `max` (didn't see that coming, did you?). Using these parameters, `minmax()` - per MDN - "defines a size range greater than or equal to min and less than or equal to max." So it's basically just `min-width` and `max-width` coming together to form a super CSS robot. _[insert Voltron gif]_

A few examples:
- `grid-template-columns: 500px minmax(300px, 500px)`: The first column will be an inflexible 500px at all time. The second column will be flexible, with a minimum width of 300px and a maximum width of 500px.

- `grid-template-columns: 500px minmax(300px, 1fr)`: The first column will be an inflexible 500px at all time. The second column will have a flexible width, growing as much as the remaining space allows, but it will never shrink below 300px.

- `grid-template-columns: 500px minmax(min-content, 500px)`: The first column will be an inflexible 500px at all times. The second column will have a minimum width of whatever the _smallest_ size of the content can be (in this case, the text will wrap); but the second column will never grow larger than 500px.

- `grid-template-columns: 500px minmax(max-content, 500px)`: The first column will be an inflexible 500px at all times. The second column will have a minimum width of whatever the _largest_ size of the content can be (in this case, the text will _not_ wrap); but the second column will never grow larger than 500px.

But wait. There's more.

### auto-Fill vs auto-Fit
Let's talk about 2 new keywords: `auto-fill` and `auto-fit`. 

- `auto-fill` - create a NEW implicit column whenever there's room for one. Even if there's no content in these new columns, they will still take up space.

- `auto-fit` - make the CURRENT columns (i.e. columns with actual content) expand to take up available space. Create new columns but collapse them to 0px width.

Both create new columns, but `auto-fit` collapses columns without content to 0px width, then stretches the columns WITH content to take up any extra space.

This is easier to understand if you can visualize what's happening _[include Codepen example, maybe just alter the same one?]_

> If you ever want a refresher, I recommend [this short video](https://www.youtube.com/watch?v=uBFEryQY1fU). It's short and straightforward.

A bit confusing, I know. But for our purposes, we're just going to use `auto-fit`, and you'll see it's practical benefit in a second.

### CSSorcery
What if I told you that by combining `repeat()` with `minmax()` and `auto-fit`, we can get a fully responsive grid without any media queries at all?

Let's go back to our original 5 items. On our grid, let's write `grid-template-columns: repeat(auto-fit, minmax(20rem, 1fr))`.

Our grid is now made up of 5 columns, each with a minimum width of `20rem` (200px) and a maximum width of `1fr`, which means it can grow beyond 200px but won't shrink below 200px. The `auto-fit` keyword ensures that no matter how our grid resizes, the columns will fill it. There won't be any gaps. Now watch what happens when we resize the window.

_Magic._

We went from 5 columns... to 4 columns... to 3 columns... to 2 columns... to 1 column. And we didn't write a single media query.

Does this get rid of the need for media queries entirely? No. 

_[Wes Bos quote about new tech not replacing old tech, just expanding our toolbox]_

But it will drastically reduce the number of media queries you need, and it makes resizing from desktop to mobile and vice versa not just painless, but practically euphoric.

If that doesn't get you excited about Grid, I don't know what will.

## Other Considerations
- Subgrid
- Browser support
- Accessibility

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
