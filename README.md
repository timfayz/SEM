# SEM

> Writing CSS code *the smart way*

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/timfayz/elementcss?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

**SEM** [sɛ:m] is a methodology for writing and organizing CSS code. It stands for: "**s**ets of **e**lements and **m**ixes". The main idea behind SEM is to create *one-property* classes (`.elements {}`), *combine* them into compound ones (`._mixes {}`) and form *sets* of both. SEM is not a mock to [BEM](https://tech.yandex.com/bem/) or any other methodologies like [ACSS](https://acss.io/), [SMACSS](https://smacss.com/), [OOCSS](https://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/), etc. Probably, it has similarities but introduce different ideas and features. For more information please go to the [Docs](#documentation) section.

## Preface
SEM is born as a consequence of the development of [elementcss](https://github.com/timfayz/elementcss) - Sass library for fast GUI prototyping. It took over than 2 years before rules, terms and key concepts are determined by the practice. As a result, methodology not only introduce some new level of abstraction, but make it possible to share CSS code, scale indefinitely, style document in WYSIWYG-like editor and work with new coming Web Components in convenient way.

## Documentation
- [Guide](docs/guide.md). Step-by-step guide into SEM
- [Conventions](docs/conventions.md). SEM conventions for naming and organizing CSS code 
- [SEM + Sass = ❤](docs/sem_and_sass.md). How to get most of using SEM and [Sass](https://sass-lang.com/)

## Examples
Once you look at the code, the [Guide](docs/guide.md) might become just a complement to your intuitive feelings. So before going deeper, I strongly recommend just to walk through one of the examples:

- [index.html](examples/index.html) - "Hello world" example ([preview](http://htmlpreview.github.com/?https://github.com/timfayz/SEM/blob/master/examples/index.html))
- [index.html](https://github.com/timfayz/timfayz.github.io/blob/master/index.html) + [main.css](https://github.com/timfayz/timfayz.github.io/blob/master/styles/main.css) - Personal webpage ([preview](https://timfayz.github.io))
- [index.html](https://github.com/marynicole/marynicole.github.io/blob/master/index.html) + [main.css](https://github.com/marynicole/marynicole.github.io/blob/master/css/main.css) - Birthday gift card ([preview](https://marynicole.github.io))



## Contributing?
If you have time and find SEM useful, feel free to open an issue for whatever reason you have: typos, contradictions, better ways to explain, organize, etc. Thank you!
