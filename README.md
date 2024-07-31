# SEM

> The scalable way to write UI

**SEM** [sɛ:m] is a CSS coding methodology that stands for "**S**ets of **E**lements and **M**ixes". SEM simplifies CSS development by following three key steps:

1. Use single-property classes (**e**lements):
   ```css
   .element1 { property: value; }
   .element2 { property: value; }
   ```

2. Combine them into **m**ixes when necessary:
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
SEM is not an imitation of [BEM](https://tech.yandex.com/bem/) or other methodologies like [ACSS](https://acss.io/), [SMACSS](https://smacss.com/), or [OOCSS](https://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/). It introduces a different approach to organizing simple, highly scalable GUIs. SEM originated from the development of [elementcss](https://github.com/timfayz/elementcss), a Sass library for rapid GUI prototyping. Over two years, rules, terms, and key concepts were refined through practical experience. Consequently, this methodology not only introduces a new level of abstraction but also facilitates code sharing, indefinite scalability, and WYSIWYG-like styling.

## Documentation
- [Guide](docs/guide.md): Step-by-step introduction to SEM
- [Conventions](docs/conventions.md): SEM conventions for naming and organizing CSS code
- [SEM + Sass = ❤](docs/sem_and_sass.md): Maximizing the benefits of using SEM and [Sass](https://sass-lang.com/)

## Examples
Before exploring the details, I recommend reviewing the examples. These serve as practical introductions, providing an understanding of SEM in action:

- [index.html](examples/index.html): "Hello world" example ([preview](http://htmlpreview.github.io/?https://github.com/timfayz/SEM/blob/master/examples/index.html))
- [index.html](https://github.com/timfayz/timfayz.github.io/blob/master/index.html) + [main.css](https://github.com/timfayz/timfayz.github.io/blob/master/styles/main.css): Personal webpage ([preview](https://timfayz.github.io))
- [index.html](https://github.com/marynicole/marynicole.github.io/blob/master/index.html) + [main.css](https://github.com/marynicole/marynicole.github.io/blob/master/css/main.css): Birthday gift card ([preview](https://marynicole.github.io))

## Contributing
Please feel free to report any issues, suggest improvements, and point out typos.
