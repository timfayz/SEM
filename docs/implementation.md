# Implementation
As it mentioned at the beginning, SEM methodology was born as a consequence of [elementcss](https://github.com/timfayz/elementcss) development or better to say on top of preprocessor. At this page we will consider how SEM usage can be greatly extended using preprocessor like Sass. This article for more advanced usage of SEM and before going further you should at least read [Explanation]() or [Key concepts]().

## Preface
Recall the previous sections. Up to this moment we haven't considered preprocessor as a helpful tool to create and manage sets of elements or mixes. Instead we used Sass only to create mixes by `extend`ing elements and to split stylesheets into logical parts. Here we will consider how we can use Sass to **generate** sets rather than *type them manually* in plain CSS. We will see how using almost one Sass mixin we could manage sets on absolutely different level - in the form in which SEM initially was intended for. Perhaps the same can be achieved in Stylus or LESS, but I didn't have a time to try it, so we stick on Sass.

## Problems using plain CSS
The main problem of using SEM in plain CSS is we have to do a lot typing and managing `@media` rules manually. For example:
```CSS
/*
 * Display set
 */
.dsp-block  {display: block;}
.dsp-none   {display: none;}
@media only screen and (max-width: 780px) {
  .mobile-dsp-block  {display: block;}
  .mobile-dsp-none   {display: none;}
}

/*
 * Visibility set
 */
.vsb-hidden  {visibility: hidden;}
@media only screen and (max-width: 780px) {
  .mobile-vsb-hidden  {visibility: hidden;}
}

/*
 * Padding set
 */
/* sm */
.pdd-sm       {padding: 5px;}
.pdd-t-sm     {padding-top: 5px;}
/* md */
.pdd-md       {padding: 10px;}
.pdd-t-md     {padding-top: 10px;}
...
```
If we look closer we can catch a several problems:

- we forced to type repeating shortcuts for element classes `.dsp-, .dsp-, .dsp-`;
- we forced to type much of repeating property declaration `{display:.., {display:..`;
- we forced to duplicate `@media` rules to visually separate different sets;
- query prefixes like `.mobile-` as well as media queries itself can be renamed or removed in the future;
- media types values like `max-width: 780px;` can be changed and we will be forced to replace them manually;

But we can avoid these problems by creating multipurpose mixin to generate sets rather that type each element manually.

## Solution using Sass
Let's consider we have a `set()` mixin:
```SCSS
@mixin set($shortcut, $data, $query-map:null) {
  // implementation
}
```
It can generate elements (as we go further mixins as well) in much more concise and manageable form. For example:
```
/*
 * Display set
 */
@include set(dsp, (
  block, block,
  none, none,
);

/*
 * Padding set
 */
@include set(pdd, (
  sm, 5px,
  md, 10px,
  lg, 20px,
);

// will be compiled into

.dsp-block {display: block;}
.dsp-none  {display: none;}

.pdd-sm {padding:5px;}
.pdd-md {padding:10px;}
.pdd-lg {padding:20px;}
```
It's the most simple example how `set()` can be used. We will update this example as we progress.
```
// There is how arguments should looks like:
$shortcut: 'pdd'; // 'brd', 'mrg', 'bg' etc.
$selector-type: '.'; // '#', '%', '', etc.
$elements: (postfix-name, value, postfix-name, value, postfix-name, value, ...);
```
