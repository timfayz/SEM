# SEM Explanation
This page is step-by-step guide to using SEM. It explains design consideration, key concepts, workflow, name conventions and file organization in depth. Please, read it carefully to the end before make any conclusions.

### Table of content
0. [Key concepts](#key-concepts)
0. [Basic workflow](#workflow)
0. [Design consideration](#design-consideration)

The guide is written so that we go from simple to complex where each section complements the previous one.

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
You can think of elements like an atomic modifiers of the whole page layout. It's worth noting that class names start with a *valid CSS property name*. The downside of the decision is class names too long! Nobody likes type too long names. And to solve this issue we introduce **3 simple rules for abbreviating all property names**. Let's skip the details and see the final result:
```CSS
.pdd-sm {padding: 5px;}

.clr-black {color: #000;}

.bg-white {background: #fff;}
```
Ok, looks better. Take into account two things: (a) despite of `pdd`, `clr`, and `bg` are *shortcuts* they're considered as a *valid* property names; (b) **each element should be written in one line** to take up less space. The shortening rule-set guarantees almost no collisions between shortcuts as well as provide good readability, brevity and intuitiveness. The test on shortening 220 properties showed only about 7 collisions. Looking ahead having short and standardized prefixes is important for the several reasons:

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
For now you can think of mixes like a regular classes as we usually do (`.header`, `.footer` etc). Underscore is used to visually separate one property classes and the combination of several ones. The decision was made because `_abstract-name` isn't really self-descriptive and can imply anything like a "black-box" when reading other people's HTML code or even your own after quite more time. Other important details around mix we discuss later. Move on.

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
 - the latter makes it possible to *rewrite styles* of any mix by element

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
 * https://www.google.ru/design/spec/style/color.html#color-color-palette */

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
.bg-blue50                {background: #03A9F4;}
.bg-blue40--hover:hover   {background: #29B6F6;}
.bg-green50               {background: #4CAF50;}
.bg-green40--hover:hover  {background: #66BB6A;}
.bg-white                 {background: #fff;}
```
It may looks scary, but don't hurry with the conclusions. You may notice elements aren't followed randomly, but organized:

>
- at the beginning sets with box-model properties, then visual related ones;
- margin & padding sets are strict *subsets* of graduated values (small, medium etc);
- background set has pseudo-class elements whose names follow after double dash `*--hover`;

So here is another important thing about SEM - **subset**. It means a **group of related elements united by some basis value under a set**. For example:
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
Optionally, **subsets can be titled by one-line comment** to visually separate related groups. Importantly, subsets should *always* arise when we talk about *well-organized sets* and we want to design truly reusable and intuitive code in SEM-way.

In this way, I want to emphasize that creating elements isn't *chaotic and messy* process. In contrast, it takes some patience, premeditate and convention. As we learn later SEM is about *building well-designed sets* of elements or mixes. In this process we will be helped by conventions we learn later. But for now skip the details and move closer to the point.

### "Hello world" example
So, take a look more carefully at the classes we have created in the previous "real-life" example. You may notice we can already "draw" some GUI-elements. For example: blockquote, buttons (blue, green), successful/warning/error messages, inputs etc. Let's create some of them:
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
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hover">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hover">button</div>
</body>
...
```
![screenshot from 2015-10-06 19 47 14](https://cloud.githubusercontent.com/assets/6209197/10315575/c710f562-6c63-11e5-9a45-d59c4039dcd2.png)<br>
[Demo](http://jsfiddle.net/snfvk69t/3/). Ok, looks pretty good :) This is how SEM *may* look like in real apps. It may seems the code are messy and verbose, but it are matter of your habit and HTML organization in general (which we optimize using Web Components a bit later). From now on I want to note some interesting assumptions:

>
- The more elements we create, the less elements we will need in the future;
- The more elements we have, the more unique GUI-elements we can "draw" without creating new classes;
- The more strictly we organize elements, the more opportunities we have to reuse our or read the others code.

At this point we've learned how to create simple GUI using SEM without creating mixes. As I mentioned earlier, SEM is about *building well-organized sets*. So ideally we can avoid the use of mixes if we design good HTML structure and sets of elements. But in practice sometimes it seems impossible to do so.

### Organizing mixes
Before going deeper go back to mixes and wonder again: why we need them at all? Or conversely, why we just can't use *only* mixes and create classes like `.header`, `.footer`, `.button` etc in familiar way? TThe answer is because the basic purpose of mixes is create when we can do the same with elements.


## Advanced Workflow
Before going further we need to introduce one of CSS preprocessors, because originally SEM is born using LESS and then Sass. Preprocessors provide some important features for SEM: `mixin`s, `@import`ing files and `extend`ing properties from one selector to another. If you don't use preprocessors anyway go to [SEM and plain CSS]()

### Organizing mixes using Preprocessor
No offense, but in the following examples we stick on [Sass](http://sass-lang.com/guide). As you remember we can `extend` selectors as follows:
```SCSS
/* [main.scss] */
._mix {
  @extend .element-1;
  @extend .element-2;
}

.element-1 {color: #fff;}
.element-2 {background: blue;}

/* compiles into */

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
  @extend .bg-green40--hover;
}

._button-blue {
  @extend .brcl-blue60;
  @extend .bg-blue50;
  @extend .bg-blue40--hover;
}

/*
 * Display set
 */
...
```
Now `._blockquote` is like shortcut for these five classes or in other words - *mix of 5 elements*. Let's apply it:
```HTML
<body class="clr-black">
 <!-- blockquote -->
 <div class="_blockquote">blockquote</div>

 <!-- blue and green buttons -->
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hover">button</div>
 <div class="dsp-inbl pdd-sm clr-white brd-thin brcl-green60 bg-green50 bg-green40--hover">button</div>
</body>
```
You can ask why mixes should? Because if mixes will mainly composed of elements changing the original elements will affect the whole document style. So this is a solution to create one-level hierarchy - mixes as objects on top of/inheriting from elements. Let's make a little tree visualization:
```
  x     x                   - - - mixes on top of mixes isn't good idea
 /       \
._button  ._input           - - - mixes as one level hierarchy
  ^ ^ ^    ^   ^
 /   \_\_ /_ _  \
/       \/     \ \
.bg-blue .pdd-sm .brd-thin  - - - elements
```

### Recomendations on using elements and mixes
Looks good, but this follows several problems. Sometimes we need to use

let's list when we shouldn't use elements
Now we need to state that ideally we should use elements as much as possible. If we can't or need to shorter long line of elements in HTML - use mixes.


### SEM and plain CSS
What if I don't use any preprocessors? If so use mixes as less as possible. Organize HTML code into separate modules using Web Components (see Polymer project(link))

### Benefits of using SEM
If you grasp the idea from previous examples you may guess SEM

### Intentionally missed
In addition SEM has recommendations for reduction postfix names. Add one more class and shorten the names according to recommendations:

### SEM and elementcss

### Troubleshooting
You may notice that class attributes `<div class="long-line..">` become much longer. And sometimes it becomes difficult to manage them. Let's examine root cases when it happens:

>
0. If we write static pages. Roughly speaking we can't create reusable GUI-templates at the server-side:
```html+php
// bad code, don't try to repeat it! :)
<?php
$_blockqoute = '<div class="pdd-l-sm mrg-t-sm mrg-b-sm clr-gray50 brd-l-thin brcl-black">%s</div>'
?>
<body "clr-black">
  <?php printf($_blockqoute, "hello"); ?>
</body>
...
```
0. If we write static pages. Roughly speaking we can't create reusable GUI-templates at the server-side:

There is no concept of *subset* for mixes. Because separating logic it's up to you how to do it better.
