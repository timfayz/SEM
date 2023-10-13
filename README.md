# SEM

> The portable way to write CSS 

**SEM** [sɛ:m] is a CSS coding methodology that stands for "**S**ets of **E**lements and **M**ixes". SEM simplifies CSS development by following three key steps:
 
1. Use only one-property classes (**e**lements):
```css
.element1 { property: value; }
.element2 { property: value; }
```

2. If necessary, combine them into **m**ixes:
```css
._mix {
  .element1, .element2;
}
```

3. Organize **s**ets (sorted by property name): 
```css
/* Set */
.p-1 { padding: 1rem; }                /* Element */
.p-2 { padding: 2rem; }                /* Element */

/* Set */
.c-blue { color: blue; }               /* Element */
.c-red { color: red; }                 /* Element */

/* Set */
.bg-white { background: white; }       /* Element */

._btn { .p-1, .c-blue, .bg-white; }    /* Mix (using preprocessors) */
```

## Preface
SEM is not a mere imitation of [BEM](https://tech.yandex.com/bem/) or other methodologies like [ACSS](https://acss.io/), [SMACSS](https://smacss.com/), [OOCSS](https://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/), etc. It introduces a different approach to organizing simple, highly-scalable GUIs. SEM is born as a consequence of the development of [elementcss](https://github.com/timfayz/elementcss) - a Sass library for fast GUI prototyping. It took more than 2 years for rules, terms, and key concepts to be established through practical experience. As a result, the methodology not only introduces a new level of abstraction but also makes it possible to share CSS code, scale indefinitely, and style documents in a WYSIWYG-like way. 

## Documentation
- [Guide](docs/guide.md). Step-by-step guide into SEM
- [Conventions](docs/conventions.md). SEM conventions for naming and organizing CSS code 
- [SEM + Sass = ❤](docs/sem_and_sass.md). How to get most of using SEM and [Sass](https://sass-lang.com/)

## Examples
Before delving into the details, I highly recommend taking a moment to explore the examples. These examples serve as a practical introduction, helping you get a feel for SEM in action:

- [index.html](examples/index.html) - "Hello world" example ([preview](http://htmlpreview.github.io/?https://github.com/timfayz/SEM/blob/master/examples/index.html))
- [index.html](https://github.com/timfayz/timfayz.github.io/blob/master/index.html) + [main.css](https://github.com/timfayz/timfayz.github.io/blob/master/styles/main.css) - Personal webpage ([preview](https://timfayz.github.io))
- [index.html](https://github.com/marynicole/marynicole.github.io/blob/master/index.html) + [main.css](https://github.com/marynicole/marynicole.github.io/blob/master/css/main.css) - Birthday gift card ([preview](https://marynicole.github.io))

## Contributing
If you've found SEM helpful, please don't hesitate to open an issue for any reason: typos, suggestions, clearer explanations. Your feedback is highly appreciated. Thank you!
