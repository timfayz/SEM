# SEM Explained
This page is a step-by-step guide to SEM. It explains design considerations, key concepts, workflow, naming conventions, and file organization in depth.

### Table of Contents
1. [Key Concepts](#key-concepts)
2. [Basic Workflow](#basic-workflow)
3. [Design Considerations](#design-considerations)
4. [Advanced Workflow](#advanced-workflow)
5. [Theming](#theming)

## Key Concepts
### Element
Get started with the core of SEM - the **element**. An element is a **class with one property whose name starts with its property name**:
```CSS
.padding-sm {
  padding: 5px;
}

.color-black {
  color: #000;
}

.background-white {
  background: #fff;
}
```
You can think of elements as atomic modifiers of the whole page layout. It's worth noting that class names start with a *valid CSS property name*. The downside of this decision is that class names are too long! Nobody likes typing long names. To solve this issue, we introduce **3 simple rules for abbreviating all property names**. Let's skip the details and see the final result:
```CSS
.pdd-sm {padding: 5px;}

.clr-black {color: #000;}

.bg-white {background: #fff;}
```
Looks better. Keep in mind two things: (a) despite `pdd`, `clr`, and `bg` being *shortcuts*, they are considered *valid* property names; (b) **each element should be written on one line** to take up less space. The shortening rule-set guarantees almost no collisions between shortcuts, providing good readability, brevity, and intuitiveness. We'll consider this later, but having short and standardized prefixes is important for several reasons:

- We can see what an element does by its class name.
- We can read others' HTML code and always grasp the layout's idea.
- We can use an IDE's autocomplete function to list related elements.
- Using short names with a lot of elements makes the resulting HTML code less verbose.

### Mix
Someone might ask: what if we have a class with more than one property? Yes, in real projects, we will. So here is the second concept - **mix**. A mix is a **regular class with more than one property and whose name starts with _** (underscore):
```CSS
._button {
 color: #fff;
 background: blue;
 padding: 5px;
}

._input {
 color: #000;
 background: white;
 padding: 5px;
}
```
While you can think of mixes as regular classes (like `.header`, `.nav`, etc.), the underscore visually separates one-property classes from combinations of several properties. This decision was made because, unlike `clr-black`, the `_abstract-name` isn't really *self-descriptive*; it can imply anything like a "black box" when we read other people's HTML code or even our own after quite some time. We will discuss other important details around mixes later. 

### Set
The last basic concept is **set**. A set is a **group of elements or mixes**. Let's add the previous mixes, some more elements, and see the result:
```CSS
/*
 * Forms set
 */
._button {
 color: #fff;
 background: blue;
 padding: 5px;
}
._input {
 color: #000;
 background: white;
 padding: 5px;
}

/*
 * Color set
 */
.clr-white  {color: #fff;}
.clr-black  {color: #000;}

/*
 * Background set
 */
.bg-blue    {background: blue;}
.bg-white   {background: white;}

/*
 * Padding set
 */
.pdd-sm     {padding: 5px;}
.pdd-md     {padding: 10px;}
```
In total, we have *4 sets*, *2 mixes*, and *6 elements*. Nothing special here, except **each set is titled by a comment header**, and **elements come after mixes**. These aren't strict rules, but:

- The former is useful to visually separate a large number of different sets.
- The latter makes it possible to *rewrite styles* of any mix by an element (like a global modifier).

*It's good practice to add spaces between curly brackets `{ property: value; }` for one-line declarations, but in SEM there are no strict rules for this. However, I prefer not to do so because managing them takes time and gives no significant advantages.*

### Summary
Before we go further, let's sum up:

- An **element** is a one-property class whose name starts with its property name abbreviated by some rules.
- A **mix** is a class with more than one property and whose name starts with an underscore.
- A **set** is a group of related elements or mixes under a header.

Now you have an idea of how SEM looks. At this step, you might ask: so what? Do we need to create dozens of one-property classes to create a complex layout? Looking ahead, the answer is yes. Now you might add: if so, what about mixes? What is special about them, and how do I know when it is better to create a mix or an element? But let's not get ahead of ourselves and focus on elements for a while. I will explain how to organize them and why it's worth dealing with dozens of them.

## Basic workflow

### Organizing elements
Let's take a look at a more real-life but still very minimalistic example:
```CSS
/* 40, 50, 60 - it's shortened numbers of 400, 500, 600 etc (tones of color).
 * Eg: https://www.google.com/design/spec/style/color.html#color-color-palette */

/*
 * Display set
 */
.dsp-inbl {display: inline-block;}

/*
 * Padding set
 */
.pdd-sm       {padding: 5px;}
.pdd-t-sm     {padding-top: 5px;}
.pdd-r-sm     {padding-right: 5px;}
.pdd-b-sm     {padding-bottom: 5px;}
.pdd-l-sm     {padding-left: 5px;}

/*
 * Margin set
 */
.mrg-sm       {margin: 5px;}
.mrg-t-sm     {margin-top: 5px;}
.mrg-r-sm     {margin-right: 5px;}
.mrg-b-sm     {margin-bottom: 5px;}
.mrg-l-sm     {margin-left: 5px;}

/*
 * Color set
 */
.clr-yellow50 {color: #FFEB3B;}
.clr-green50  {color: #8BC34A;}
.clr-red50    {color: #F44336;}
.clr-gray50   {color: #757575;}
.clr-white    {color: #fff;}
.clr-black    {color: #000;}

/*
 * Border set
 */
.brd-thin     {border: 1px solid;}
.brd-t-thin   {border-top: 1px solid;}
.brd-r-thin   {border-right: 1px solid;}
.brd-b-thin   {border-bottom: 1px solid;}
.brd-l-thin   {border-left: 1px solid;}

/*
 * Border-color set
 */
.brcl-black   {border-color: #000;}
.brcl-blue60  {border-color: #039BE5;}
.brcl-green60 {border-color: #7CB342;}

/*
 * Background set
 */
.bg-blue50              {background: #03A9F4;}
.bg-blue40--hvr:hover   {background: #29B6F6;}
.bg-green50             {background: #4CAF50;}
.bg-green40--hvr:hover  {background: #66BB6A;}
.bg-white               {background: #fff;}
```

You may notice that the order of elements is not random:

- At the beginning, sets with box-model properties are followed by visual-related ones.
- Margin and padding sets are strict *subsets* of graduated values (small, medium, etc.).
- The background set has pseudo-class elements whose names follow a double dash `*--hvr`.

Another important aspect of SEM is **subsets**. A subset is a **group of related elements or mixes under a set** united by a common value. For example:

```CSS
/*
 * Padding set
 */
/* sm */
.pdd-sm       {padding: 5px;}             ^
.pdd-t-sm     {padding-top: 5px;}         | "sm" subset
...                                       v
/* md */
.pdd-md       {padding: 10px;}            ^
.pdd-t-md     {padding-top: 10px;}        | "md" subset
...                                       v

/*
 * Border set
 */
/* thin */
.brd-thin     {border: 1px solid;}        ^
.brd-t-thin   {border-top: 1px solid;}    | "thin" subset
...                                       v
/* thick */
.brd-thick    {border: 3px solid;}        ^
.brd-t-thick  {border-top: 3px solid;}    | "thick" subset
...                                       v

/*
 * Border-color set
 */
.brcl-black    {border-color: #000;}      ^
.brcl-blue60   {border-color: #039BE5;}   | Set is a subset itself
.brcl-green60  {border-color: #7CB342;}   v
```

Optionally, **subsets can be titled by a one-line comment** to visually separate related groups. Importantly, subsets should *always* arise when we talk about well-designed sets or want to design truly reusable and intuitive code in the SEM way.

This emphasizes that creating elements isn't a *chaotic and messy* process. On the contrary, it requires patience and forethought. As we learn later, SEM is about *building well-designed sets*. Conventions we will discuss later will help in this process. For now, let's move closer to the point.

### "Hello World" Example

Take a closer look at the classes we created in the previous "real-life" example. You'll notice we can already "draw" some GUI elements, such as blockquotes, buttons (blue, green), success/warning/error messages, inputs, etc. Let's create some of them and see how SEM *may* look in action:

```HTML
...
<body class="clr-black">
 <!-- Blockquote -->
 <div class="pdd-l-sm mrg-t-sm mrg-b-sm clr-gray50 brd-l-thin brcl-black">blockquote</div>

 <!-- Input -->
 <input type="text" class="dsp-inbl pdd-sm brd-thin brcl-black bg-white"/><br>

 <!-- Messages -->
 <div class="dsp-inbl pdd-sm clr-green50">Successful</div>,
 <div class="dsp-inbl pdd-sm clr-yellow50">Warning</div>,
 <div class="dsp-inbl pdd-sm clr-red50">Error</div>

 <!-- Horizontal Line -->
 <div class="pdd-t-sm mrg-b-sm brd-b-thin brcl-black"></div>

 <!-- Blue (Primary) and Green Buttons -->
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hvr">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hvr">button</div>
</body>
...
```

![screenshot from 2015-10-06 19 47 14](https://cloud.githubusercontent.com/assets/6209197/10315575/c710f562-6c63-11e5-9a45-d59c4039dcd2.png)<br>
[Demo](http://jsfiddle.net/snfvk69t/3/). Looks pretty good :) This example demonstrates how we can *divide* our design into small logical pieces. By putting them together, we can create not only what we've designed but also *unlimited variations* of future ideas in a *similar form*. It may seem that the code is messy and verbose, but it's a matter of habit and HTML organization in general (which we optimize using Web Components a bit later). Here are some interesting observations:

- The more elements we create, the fewer elements we will need in the future.
- The more elements we have, the more unique GUI elements we can "draw" without creating new classes.
- The more strictly we organize elements, the more opportunities we have to reuse our code or understand others' code.

At this point, we've learned how to create a simple GUI using SEM without creating mixes. As mentioned earlier, SEM is about *building well-designed sets*. Ideally, we can avoid the use of mixes if we design a good HTML structure and sets of elements. However, in practice, it seems impossible to do so, hence the introduction of mixes.

### Organizing Mixes

Before going deeper, let's ask again: why do we need mixes at all? Or conversely, why can't we just use *only* mixes and create classes like `._nav`, `._footer`, and `._button` in a familiar way? The answer lies in the purposes of mixes. It has been discovered that we need them when:

1. We create specific styles that appear in the design only once.
2. It's more rational to create *indivisible styles*.
3. We create a *shortcut* of several elements to type less. See [Advanced workflow]().

Let's talk about *indivisible styles*. [Example]

```CSS
._icon, _icon::before {
  display: inline-block;
  font-family: font-icon;
  font-style: normal;
  font-variant: normal;
  font-weight: normal;
  font-size: inherit;
  line-height: inherit;
  text-rendering: auto;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
  font-feature-settings: 'liga';
}
.cnt-home--before::before {
  content: "home";
}
```

If you subsequently discover that you have elements that can replace a mix, do it and get rid of the mix. If not, leave it alone.

## Advanced Workflow

Before going further, we need to introduce one of the CSS preprocessors because SEM originally started with [LESS](https://lesscss.org) and then moved to [Sass](https://sass-lang.com). Preprocessors provide important features for SEM: `mixins` (similar to functions in programming languages), `@import` for files, and `extend` for properties from one selector to another. If you have no pre-installed packages, you can play with it online: [Sass](http://sassmeister.com/gist/fa30826e11bc858c9c93), [LESS](http://lesscss.org/less-preview/#%7B%22less%22%3A%22._mix%20%7B%5Cn%20%20%26%3Aextend(.element-1)%3B%5Cn%20%20%26%3Aextend(.element-2)%3B%5Cn%7D%5Cn%5Cn.element-1%20%7Bcolor%3A%20%23fff%3B%7D%5Cn.element-2%20%7Bbackground%3A%20blue%3B%7D%5Cn%5Cn%22%7D), [Stylus](https://learnboost.github.io/stylus/try.html). If you don't want to use preprocessors, go to [SEM and plain CSS](). In the next examples, we will stick with [Sass](https://sass-lang.com/guide). You can do the same using LESS or Stylus. The only exception with Stylus is that it cannot `@extend` selectors prior to their declaration.

### Organizing Mixes Using a Preprocessor

As you remember, we can `extend` selectors as follows:

```SCSS
/* [main.scss] */
._mix {
  @extend .element-1;
  @extend .element-2;
}

.element-1 {color: #fff;}
.element-2 {background: blue;}

/* >> compiles into >> */

/* [main.css] */
.element-1, ._mix {color: #fff;}
.element-2, ._mix {background: blue;}
```

Now we need to introduce one of the key principles about mixes - **mixes should be made of elements** or, in other words, consist of them. SHOULD means *as much as possible*. Let's apply this rule to our previous real-life example:

```SCSS
/*
 * GUI-elements set
 */
._blockquote {
  @extend .pdd-l-sm;
  @extend .mrg-b-sm;
  @extend .clr-gray50;
  @extend .brd-l-thin;
  @extend .brcl-black;
}

._button {
  @extend .dsp-inbl;
  @extend .pdd-sm;
  @extend .clr-white;
  @extend .brd-thin;
}

._button-green {
  @extend .brcl-green60;
  @extend .bg-green50;
  @extend .bg-green40--hvr;
}

._button-blue {
  @extend .brcl-blue60;
  @extend .bg-blue50;
  @extend .bg-blue40--hvr;
}

/*
 * Display set
 */
...
```

If we apply `._blockquote` to a tag, it will do the same as if we applied the classes it contains directly. So in other words, `._blockquote` is a *mix of 5 elements*:

```HTML
before
<body class="clr-black">
 <!-- Blockquote -->
 <div class="pdd-l-sm mrg-t-sm mrg-b-sm clr-gray50 brd-l-thin brcl-black">blockquote</div>
 ...
 <!-- Blue and Green Buttons -->
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hvr">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hvr">button</div>
</body>

after
<body class="clr-black">
 <!-- Blockquote -->
 <div class="_blockquote">blockquote</div>
 ...
 <!-- Blue and Green Buttons -->
 <div class="_button _button-green">button</div>
 <div class="_button _button-blue">button</div>
</body>
```

It may seem like we are designing a layout using regular and *fat* classes (like BEM, OOCSS, etc.), but actually, they are just *shortcuts* and can be removed if necessary. You might ask why mixes should consist of elements. Because if mixes are mainly composed of elements, changing the original will affect the whole layout. This gives flexibility or "mobility" to your code. And now we can state the general idea behind SEM - *make less abstraction to get less overhead in the future*. We can visualize it as follows:

```
  x     x                   <-- mixes on top of mixes isn't a good idea
 /       \
._button  ._input           <-- mixes as a single-level abstraction
  ^ ^ ^    ^   ^
 /   \_\_ /_ _  \
/       \/     \ \
.bg-blue .pdd-sm .brd-thin  <-- elements
```

Mixes can be applied for creating one-level hierarchies or *indivisible UI elements* (e.g. a button). There is no concept of *subsets* for mixes. The separation of logic and structure is up to you.

## Recommendations on Using Elements and Mixes

Consider when you shouldn't create elements:

[WIP]

When adding new elements, ask yourself:

- Is it worth adding a new element?
- Can I add it to an existing subset?
- Can I create a new subset with it and some randomly created elements?
- Can I redesign an existing subset so that the need for a new one is suppressed?

Ideally, we should use elements as much as possible. If we can't or need to shorten long lines of elements in HTML, use mixes.

### SEM and Plain CSS

Using SEM and plain CSS has several limitations. General recommendations are:

- Use mixes as little as possible.
- Organize HTML code into separate modules using Web Components (see the Polymer project(link)).
- [WIP]

### Theming
[WIP]

### Design Considerations
[WIP]

### Shortening Properties

The test on shortening 220 properties showed only about 7 collisions. Let's look at shortening syntax in detail:

- One word property = first letter and two subsequent consonants:
  ```
  [p]o[s]i[t]ion  = pst
  [o][v]e[r]flow  = ovr
  ```
- Two word property = first letter, one subsequent consonant, and the same for the second word:
  ```
  [b]o[x]-[s][h]adow  = bxsh
  [f]o[n]t-[s]i[z]e   = fnsz
  ```
- Three word property = first letter, one subsequent consonant, and the first two letters of the subsequent words:
  ```
  [a][n]imation-[t]iming-[f]unction = antf
  [b]o[r]der-[i]mage-[s]ource       = bris
  ```

Property direction is abbreviated as follows:

```
  -top      = -t
  -top-left = -tl
  -bottom   = -b
  -x/-y     = -x/-y
```

Property direction is considered as a postfix of a class name and always written separately at the end using a "-" hyphen:

```
  border-top-color        = [b]o[r]der(-top-)[c]o[l]or        = brcl-t
  border-top-left-radius  = [b]o[r]der(-top-left-)[r]a[d]ius  = brrd-tl
  margin-bottom           = [m]a[r][g]in(-bottom)             = mrg-b
  overflow-x              = [o][v]e[r]flow(-x)                = ovr-x
```

### Shortening postfixes
In addition, SEM has recommendations for reducing postfix names. See [Conventions]().
