#SEM Explanation
This page is step-by-step guide to using SEM. It explains the key concepts, name conventions and file organization in depth. Please, read it carefully to the end before make any conclusions.

###Element
Get started with the core of SEM - **element**. Element is a **class with one property whose name starts with its property name**:
```CSS
.padding-md {
  padding: 10px;
}

.color-black {
  color: #000;
}

.background-clouds {
  background: url('clouds.jpg');
}
```
It's worth noting that element class names start with a *valid CSS property name*. The downside of the decision is class names too long! Nobody likes type too long names. And to solve this issue we introduce **3 simple rules for abbreviating all property names**. Let's skip the details and see the final result:
```CSS
.pdd-md {padding: 10px;}

.c-black {color: #000;}

.bg-clouds {background: url('clouds.jpg');}
```
Ok, looks better. Take into account two things: (a) despite of `pdd`, `c`, and `bg` are *shortcuts* they're considered as a *valid* property names in context of element; (b) **each element should be written in one line** to take up less space. The designed shortening rule-set guarantees almost no collisions between shortcuts as well as provide good readability, brevity and intuitiveness. The test on 230 CSS properties showed only about 7 collisions. Looking ahead having short and standardized prefixes is important for the several reasons:

>
- we can see what element does by its class name;
- we can read others code and always grasp the general idea of layout;
- we can use IDE's autocomplete function to list related elements;
- using short names and a lot of elements make resulting HTML code less verbose;

###Mix
Someone can ask: what if we have classes with more than one property? Yes, in real projects we will. So here is the second concept - **mix**. Mix is a **regular class with more than one property and whose name starts with _** (underscore):
```CSS
._input {
  color: #000;
  background: url('clouds.jpg');
  padding: 10px;
}

._button {
  color: #fff;
  background: blue;
  padding: 5px;
}
```
Underscore is used to visually separate classes with one property and the mix of several ones. The decision was made because `._abstract-name` isn't really self-descriptive and can imply anything like a "black-box" when reading other people's HTML code or even your own after quite more time. Other important details around mix we discuss later. Move on.

###Set
Here is the last main concept - **set**. Set is a **group of elements or mixes**. Add the previous mixes, some more elements and see the result:
```CSS
/*
 * Color set
 */
.c-white  {color: #fff;}
.c-black  {color: #000;}

/*
 * Background set
 */
.bg-blue   {background: blue;}
.bg-clouds {background: url('clouds.jpg');}

/*
 * Padding set
 */
.pdd-sm {padding: 5px;}
.pdd-md {padding: 10px;}

/*
 * Forms set
 */
._input {
  color: #000;
  background: url('clouds.jpg');
  padding: 10px;
}
._button {
  color: #fff;
  background: blue;
  padding: 5px;
}
```
Nothing special here, except **each set is titled by comment header**. In total we have *4 sets*, *6 elements* and *2 mixes*.

*It's a good practice to add spaces between curly brackets `{ property: value; }` for one-line declarations, but in SEM there is no strict rules for it. However I prefer not to do so, because manage them takes time and give no significant advantages.*

###Summarize
Before we go further let's sum up:

- **element** is a class with one property and whose name is property name abbreviated by some rules;
- **mix** is a class with more than one property and whose name start with underscore;
- **set** is a group of elements or mixes under one section;

Now you have an idea how SEM looks like. At this step you can ask: and so what? We need to create dozens of one property classes and miraculously create complex layout?! Looking ahead the answer is yes. Now you can add: if so what about mixes? What is special with them and how do I know when is better to create mix or element? But let's not get ahead of ourselves and focus on elements for a while. I will explain how to organize them and why it's worth coping with dozens of them.

###Organizing elements
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
.pdd-md       {padding: 10px;}
.pdd-t-md     {padding-top: 10px;}
.pdd-r-md     {padding-right: 10px;}
.pdd-b-md     {padding-bottom: 10px;}
.pdd-l-md     {padding-left: 10px;}

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
.c-yellow50   {color: #FFEB3B;}
.c-green50    {color: #8BC34A;}
.c-red50      {color: #F44336;}
.c-gray50     {color: #757575;}
.c-white      {color: #fff;}
.c-black      {color: #000;}

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
.brcl-black    {border-color: #000;}
.brcl-blue60   {border-color: #039BE5;}
.brcl-green60  {border-color: #7CB342;}

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
- margin & padding sets are strict _*collections*_ of graduated values (small, medium etc);
- background set has pseudo-class elements with an appropriate names goes after double dash `*--hover`;

Let's take a look at one more point about elements. It is subsequent concept of set - **collection**. Collection is a **group of elements united by related values under one set**. For example:
```CSS
/*
 * Padding set
 */
/* sm */
.pdd-sm       {padding: 5px;}             ^
.pdd-t-sm     {padding-top: 5px;}         | "sm" collection
.pdd-r-sm     {padding-right: 5px;}       |
...                                       v
/* md */
.pdd-md       {padding: 10px;}            ^
.pdd-t-md     {padding-top: 10px;}        | "md" collection
.pdd-r-md     {padding-right: 10px;}      |
...                                       v

/*
 * Border set
 */
/* thin */
.brd-thin     {border: 1px solid;}        ^
.brd-t-thin   {border-top: 1px solid;}    | "thin" collection
.brd-b-thin   {border-bottom: 1px solid;} |
...                                       v
/* thick */
.brd-thick    {border: 3px solid;}        ^
.brd-t-thick  {border-top: 3px solid;}    | "thick" collection
.brd-b-thick  {border-bottom: 3px solid;} |
...                                       v

/*
 * Border-color set
 */
.brcl-black    {border-color: #000;}      ^
.brcl-blue60   {border-color: #039BE5;}   | set is a collection itself
.brcl-green60  {border-color: #7CB342;}   v
```
Optionally, **collections can be titled by one-line comment**.

In this way, I want to emphasize that creating elements isn't *chaotic and messy* process. In contrast, creating elements takes some patience, premeditate and conventions. But for now skip the details and move closer to the point.

###Usage
So, if you look more carefully at the classes we have created at the penultimate example you may notice we can already "draw" some GUI-elements. For example: blockquote,  buttons (blue, green), successful/warning/error messages, input etc. Let's create some of them:
```HTML
...
<body class="c-black">
  <!-- blockquote -->
  <div class="pdd-l-sm mrg-t-sm mrg-b-sm c-gray50 brd-l-thin brcl-black">blockquote</div>

  <!-- input -->
  <input type="text" class="dsp-inbl pdd-sm brd-thin brcl-black bg-white"/><br>

  <!-- messages -->
  <div class="dsp-inbl pdd-sm c-green50">Successful</div>,
  <div class="dsp-inbl pdd-sm c-yellow50">Warning</div>,
  <div class="dsp-inbl pdd-sm c-red50">Error</div>

  <!-- horizontal line -->
  <div class="pdd-t-sm mrg-b-sm brd-b-thin brcl-black"></div>

  <!-- blue (primary) and green buttons -->
  <div class="dsp-inbl pdd-sm c-white brd-thin brcl-blue60 bg-blue50 bg-blue40--hover">button</div>
  <div class="dsp-inbl pdd-sm c-white brd-thin brcl-green60 bg-green50 bg-green40--hover">button</div>
</body>
```
![screenshot from 2015-10-06 19 47 14](https://cloud.githubusercontent.com/assets/6209197/10315575/c710f562-6c63-11e5-9a45-d59c4039dcd2.png)<br>
[Demo](http://jsfiddle.net/snfvk69t/1/). Ok, looks pretty good :) This is how SEM *may* look like in real apps. It may seems the code are messy and verbose, but it are matter of habit and HTML organization (which we optimize using Web Components a bit later). From now on I want to note some interesting assumptions:

>
- The more elements we create, the less elements we will need in the future;
- The more elements we have, the more unique GUI-elements we can "draw" without creating new classes;
- The more strictly we organize sets of elements, the more opportunities we have to reuse our code or read others.


As you can see, ....

If so why we need mixes at all? May be we can just focus on mixes and write the code in the familiar form (creating classes like `._header`, `_footer` etc). Before drawing any premature conclusions we need to introduce Sass, because initially SEM is born using Sass. [Sass](http://sass-lang.com/guide) provides some important features for SEM: `@mixin`s, `@import`ing files and `@extend`ing properties from one selector to another.

###SEM and Sass
Now we need to introduce one of the key principle - **mix should be mix of elements**. SHOULD means *as much as possible*. You can ask why? Because if mixes will mainly composed of elements changing the original elements will change the whole document style. Let's make a little tree visualization:
```
._button  ._input   - - - mixes level
   ^ ^ ^    ^   ^
  /   \_\_ /_ _  \
 /       \/     \ \
.bg-blue .pdd-sm .brd-thin  - - - elements level
```
How to orginize elements, how to organize mixes.
 One of the main idea of SEM is to style mixes by extending elements:
```SCSS
[main.scss]

/* Color set */
.c-white  {color: white;}
.c-black  {color: black;}

/* Background set */
.bg-blue   {background: blue;}
.bg-clouds {background: url('clouds.jpg');}

/* Padding set */
.pdd-sm {padding: 5px;}
.pdd-md {padding: 10px;}

/* Forms set */
._button {
  @extend .c-white;
  @extend .bg-blue;
  @extend .pdd-sm;
}
...
```
Compiled into:
```CSS
[main.css]

/* Color set */
.c-white, ._button {color: white;}
...

/* Background set */
.bg-blue, ._button {background: blue;}
...

/* Padding set */
.pdd-sm, ._button {padding: 5px;}
...
```
Now `._button` is like shortcut for these three classes or in other words - *mix of 3 elements*. Take an example how it can looks like in HTML:
```HTML
before
<form class="pdd-xsm">
  <input type="submit" class="c-white background-blue pdd-sm">
</form>

after
<form class="pdd-md">
  <input type="submit" class="_button">
</form>
```
 So this is a solution to create one-level hierarchy - mixes as objects on top of/inheriting from elements.
Looks good, but this follows several problems. Sometimes we need to use

###Intentionally missed
In addition SEM has recommendations for reduction postfix names. Add one more class and shorten the names according to recommendations:
###SEM and elementcss
