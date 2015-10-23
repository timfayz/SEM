# SEM Explanation
This page is step-by-step guide to using SEM. It explains design consideration, key concepts, workflow, name conventions and file organization in depth. Please, read it carefully to the end before make any conclusions.

### Table of content
0. [Key concepts](#key-concepts)
0. [Basic workflow](#basic-workflow)
0. [Design consideration](#design-consideration)
0. [Advanced workflow](#advanced-workflow)
0. [Theming](#theming)

The guide is written so that we go from simple to complex gradually where each section complements the previous one.

## Key concepts
### Element
Get started with the core of SEM - **element**. Element is a **class of one property whose name starts with its property name**:
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
You can think of elements like an atomic modifiers of the whole page layout. It's worth noting that class names start with a *valid CSS property name*. The downside of the decision is class names too long! Nobody likes type too long names. To solve this issue we introduce **3 simple rules for abbreviating all property names**. Let's skip the details and see the final result:
```CSS
.pdd-sm {padding: 5px;}

.clr-black {color: #000;}

.bg-white {background: #fff;}
```
Ok, looks better. Take into account two things: (a) despite of `pdd`, `clr`, and `bg` are *shortcuts* they're considered as a *valid* property names; (b) **each element should be written in one line** to take up less space. The shortening rule-set guarantees almost no collisions between shortcuts providing good readability, brevity and intuitiveness. We will consider it later, but looking ahead having short and standardized prefixes is important for the several reasons:

>
- we can see what element does by its class name;
- we can read others HTML code and always grasp the idea how layout looks like;
- we can use IDE's autocomplete function to list related elements;
- using short names with a lot of elements make resulting HTML code less verbose;

### Mix
Someone can ask: what if we have class with more than one property? Yes, in real projects we will. So here is the second concept - **mix**. Mix is a **regular class with more than one property and whose name starts with _** (underscore):
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
While you can think of mixes like a regular classes as we usually do (`.header`, `.nav` etc). Underscore is used to visually separate one property classes and the combination of several ones. The decision was made because unlike `clr-black` the `_abstract-name` isn't really *self-descriptive*; it can imply anything like a "black-box" when we read other people's HTML code or even our own after quite more time. Other important details around mix we discuss later. Move on.

### Set
Here is the last main concept - **set**. Set is a **group of elements or mixes**. Add the previous mixes, some more elements and see the result:
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
In total we have *4 sets*, *2 mixes* and *6 elements*. Nothing special here, except **each set is titled by comment header** and **elements come after mixes**. It's not strict rules, but:

>
 - the former is useful to visually separate a flow of a large number of different sets
 - the latter makes it possible to *rewrite styles* of any mix by element (like an global modifier)

*It's a good practice to add spaces between curly brackets `{ property: value; }` for one-line declarations, but in SEM there is no strict rules for it. However I prefer not to do so, because manage them takes time and give no significant advantages.*

### Summarize
Before we go further let's sum up:

- **element** is a one property class whose name starts with its property name abbreviated by some rules;
- **mix** is a class with more than one property and whose name starts with underscore;
- **set** is a group of related elements or mixes under a header;

Now you have an idea how SEM looks like. At this step you can ask: and so what? We need to create dozens of one property classes and miraculously create complex layout?! Looking ahead the answer is yes. Now you can add: if so what about mixes? What is special with them and how do I know when is better to create mix or element? But let's not get ahead of ourselves and focus on elements for a while. I will explain how to organize them and why it's worth coping with dozens of them.

## Basic workflow

### Organizing elements
Let's take a look at more "real-life" (but still very minimalistic) example:
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
It may looks scary, but don't hurry with the conclusions. You may notice elements aren't followed randomly, but organized:

>
- at the beginning sets with box-model properties, then visual related ones;
- margin & padding sets are strict *subsets* of graduated values (small, medium etc);
- background set has pseudo-class elements whose names follow after double dash `*--hvr`;

So here is another important thing about SEM - **subset**. It means a **group of related elements or mixes under a set** united by some basis value. For example:
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
.brcl-blue60   {border-color: #039BE5;}   | set is a subset itself
.brcl-green60  {border-color: #7CB342;}   v
```
Optionally, **subsets can be titled by one-line comment** to visually separate related groups. Importantly, subsets should *always* arise when we talk about well-designed sets or want to design truly reusable and intuitive code in SEM-way.

In this way, I want to emphasize that creating elements isn't *chaotic and messy* process. In contrast, it takes some patience and premeditate. As we learn later SEM is about *building well-designed sets*. In this process we will be helped by conventions we learn later. But for now skip the details and move closer to the point.

### "Hello world" example
So, take a look more carefully at the classes we have created in the previous "real-life" example. You may notice we can already "draw" some GUI-elements. For example: blockquote, buttons (blue, green), successful/warning/error messages, inputs etc. Let's create some of them and see how SEM *may* look like in action:
```HTML
...
<body class="clr-black">
 <!-- blockquote -->
 <div class="pdd-l-sm mrg-t-sm mrg-b-sm clr-gray50 brd-l-thin brcl-black">blockquote</div>

 <!-- input -->
 <input type="text" class="dsp-inbl pdd-sm brd-thin brcl-black bg-white"/><br>

 <!-- messages -->
 <div class="dsp-inbl pdd-sm clr-green50">Successful</div>,
 <div class="dsp-inbl pdd-sm clr-yellow50">Warning</div>,
 <div class="dsp-inbl pdd-sm clr-red50">Error</div>

 <!-- horizontal line -->
 <div class="pdd-t-sm mrg-b-sm brd-b-thin brcl-black"></div>

 <!-- blue (primary) and green buttons -->
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hvr">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hvr">button</div>
</body>
...
```
![screenshot from 2015-10-06 19 47 14](https://cloud.githubusercontent.com/assets/6209197/10315575/c710f562-6c63-11e5-9a45-d59c4039dcd2.png)<br>
[Demo](http://jsfiddle.net/snfvk69t/3/). Ok, looks pretty good :) The example demonstrates how we can *divide* our design into small logical pieces. Putting them together we can create not only what we've designed, but also *unlimited variations* of your future ideas in a *similar form*.  It may seems the code are messy and verbose, but it are matter of your habit and HTML organization in general (which we optimize using Web Components a bit later). From now on I want to note some interesting assumptions:

>
- The more elements we create, the less elements we will need in the future;
- The more elements we have, the more unique GUI-elements we can "draw" without creating new classes;
- The more strictly we organize elements, the more opportunities we have to reuse our or read the others code.

At this point we've learned how to create simple GUI using SEM without creating mixes. As it mentioned earlier, SEM is about *building well-designed sets*. So ideally, we can avoid the use of mixes if we design good HTML structure and sets of elements. But it seems impossible to do so in practice and mixes was introduced.

### Organizing mixes
Before going deeper let's wonder again: why we need mixes at all? Or conversely, why we just can't use *only* mixes and create classes like `._nav`, `._footer` and `._button` in familiar way? The answer is in purposes of mixes. It was discovered we need them when:

 0. we create a specific styles that appears in the design only a once.
 0. it more rationally to create an *indivisible styles*.
 0. we create a *shortcut* of several elements to type less. See [Advanced workflow]().

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
If you subsequently discovered you have an elements you can replace mix on - do it and get rid of mix, if you don't leave it alone.

## Advanced workflow
Before going further we need to introduce one of CSS preprocessors, because originally SEM is born using [LESS](lesscss.org) and then [Sass](sass-lang.com). Preprocessors provide some important features for SEM: `mixin`s, `@import`ing files and `extend`ing properties from one selector to another. If you have no installed preprocessor - play it online: [Sass](http://sassmeister.com/gist/fa30826e11bc858c9c93), [LESS](http://lesscss.org/less-preview/#%7B%22less%22%3A%22._mix%20%7B%5Cn%20%20%26%3Aextend(.element-1)%3B%5Cn%20%20%26%3Aextend(.element-2)%3B%5Cn%7D%5Cn%5Cn.element-1%20%7Bcolor%3A%20%23fff%3B%7D%5Cn.element-2%20%7Bbackground%3A%20blue%3B%7D%5Cn%5Cn%22%7D), [Stylus](https://learnboost.github.io/stylus/try.html). If you don't use preprocessors anyway go to [SEM and plain CSS](). No offense, but in the next examples we stick on [Sass](http://sass-lang.com/guide). You can do everything I'll show using LESS or Stylus as well. The one excerpt about Stylus - it can't `@extend` selectors prior to its declaration.

### Organizing mixes using Preprocessor
 As you remember we can `extend` selectors as follows:
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
Now we need to introduce one of the key principle about mixes - **mix should be made of elements** or in other words consists of them. SHOULD means *as much as possible*. Let's apply the rule on our previous "real-life" example:
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
Now if we apply `._blockquote` on a tag it will do the same as if we apply the classes it contains directly. So in other words `._blockquote` is a *mix of 5 elements*:
```HTML
before
<body class="clr-black">
 <!-- blockquote -->
 <div class="pdd-l-sm mrg-t-sm mrg-b-sm clr-gray50 brd-l-thin brcl-black">blockquote</div>
 ...
 <!-- blue and green buttons -->
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hvr">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hvr">button</div>
</body>

after
<body class="clr-black">
 <!-- blockquote -->
 <div class="_blockquote">blockquote</div>
 ...
 <!-- blue and green buttons -->
 <div class="_button _button-green">button</div>
 <div class="_button _button-blue">button</div>
</body>
```
It may seems like we design layout using regular and *fat* classes (like BEM, OOCSS etc), but actually they are just *shortcuts* and can be removed if necessary. You can ask why mixes should consist of elements? Because if mixes will mainly composed of elements changing the original will affect the whole layout. It gives flexibility or "mobility" to your code. And now we can say the general idea behind of SEM - *make less abstraction to get less overhead in the future*. We can simply visualize it as follows:
```
  x     x                   - - - mixes on top of mixes isn't good idea
 /       \
._button  ._input           - - - mixes as one level hierarchy
  ^ ^ ^    ^   ^
 /   \_\_ /_ _  \
/       \/     \ \
.bg-blue .pdd-sm .brd-thin  - - - elements
```
So mixes can be used as a solution to create one-level hierarchy (**if necessary**) or for *indivisible objects*. There is no concept of *subset* for mixes. Because separating logic it's up to you how to do it better.
[...]

### Recomendations on using elements and mixes
Let's consider when we shouldn't create elements:

>
- add

When you add new elements ask yourself:

>
- Is it worth to add new element?
- Can I add it to the existing subset?
- Can I create a new subset with it and sometime randomly created elements?
- Can I redesign existing subset so that the need of new one will be suppressed?

Ideally, we should use elements as much as possible. If we can't or need to shorter s long line of elements in HTML - use mixes.

## Intentionally missed

### SEM and plain CSS
Using SEM and plain CSS have several limitations. The general recommendations are:

>
- use mixes as less as possible;
- organize HTML code into separate modules using Web Components (see Polymer project(link))
- [more]

### Theming
[explain]

### Design consideration
[explain]

### Shortening properties
The test on shortening 220 properties showed only about 7 collisions. Now let's look at shortening syntax in details:

  - one word property = first letter and two subsequent consonants:
```
   [p]o[s]i[t]ion  = pst
   [o][v]e[r]flow  = ovr
```
  - two word property = first letter, one subsequent consonant and the same for second word:
```
   [b]o[x]-[s][h]adow  = bxsh
   [f]o[n]t-[s]i[z]e   = fnsz
```

  - three word = first letter, one subsequent consonant and two first letters of subsequent words:
```
   [a][n]imation-[t]iming-[f]unction = antf
   [b]o[r]der-[i]mage-[s]ource       = bris
```

  Property direction abbreviated as follows:
```
   -top      = -t
   -top-left = -tl
   -bottom   = -b
   -x/-y     = -x/-y
```

  Property direction is considered not as a word and always written separately at the end of shorthand using "-" hyphen:
```
   border-top-color        = [b]o[r]der(-top-)[c]o[l]or        = brcl-t
   border-top-left-radius  = [b]o[r]der(-top-left-)[r]a[d]ius  = brrd-tl
   margin-bottom           = [m]a[r][g]in(-bottom)             = mrg-b
   overflow-x              = [o][v]e[r]flow(-x)                = ovr-x
```

### Shortening postfixes
In addition SEM has recommendations for reduction postfix names. See [Conventions]().
