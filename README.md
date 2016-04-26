# JigSass Utils Spacing
[![NPM version][npm-image]][npm-url]  [![Dependency Status][daviddm-image]][daviddm-url]   

 > A collection of dynamically generated spacing utility classes

Class names follow the [Emmet](http://docs.emmet.io/cheat-sheet/) abbreviation
syntax, with colons (':') replaced by two dashes (`--`) to follow BEM naming
conventions.

#### Available classes

  - `.u-m--<amount>`: margin
  - `.u-mt--<amount>`: margin top
  - `.u-me--<amount>`: margin end (right in `LTR`, left in `RTL`)
  - `.u-mb--<amount>`: margin bottom
  - `.u-ms--<amount>`: margin start (left in `LTR`, right in `RTL`)
  - `.u-mh--<amount>`: horizontal margins (left and right)
  - `.u-mv--<amount>`: vertical margins (top and bottom)
  - `.u-mr--<amount>`: margin right
  - `.u-ml--<amount>`: margin left


  - `.u-p--<amount>`: padding
  - `.u-pt--<amount>`: padding top
  - `.u-pe--<amount>`: padding end (right in `LTR`, left in `RTL`)
  - `.u-pb--<amount>`: padding bottom
  - `.u-ps--<amount>`: padding start (left in `LTR`, right in `RTL`)
  - `.u-ph--<amount>`: horizontal padding
  - `.u-pv--<amount>`: vertical padding
  - `.u-pr--<amount>`: padding right
  - `.u-pl--<amount>`: padding left


Where `<amount>` can be either a unitless number representing a number of
typographic rhythm units as defined in
[$jigsass-sizes](https://txhawks.github.io/jigsass-tools-typography/#variable-jigsass-sizes),
a percentage, or a length specified in pixels.

JigSass Spacing supports negative margins, using `min-<number>` modifiers 
(e.g., `h-mt--min-4`), as well as auto margins (e.g., `h-m--auto`).

Additionally, JigSass Spacing provide the autospace (`.u-autospace`) set of helper classes 
for intelligently spacing an element's direct descendants:


  - `.u-autospace--<amount>`
  - `.u-autospace--<amount>(<n>up)`


`.u-autospace--<amount>` classes will give all but the first direct descendant 
of the element carrying the class a `margin-top` of `<amount>`.

Similarly, `.u-autospace--<amount>(<n>up)` classes will give all but the `n` 
direct descendants of the element carrying the class a `margin-top` of 
`<amount>`, allowing for intelligent spacing of grid-like layouts.

For an in-depth presentation of the technique, see 
[this article](http://alistapart.com/article/axiomatic-css-and-lobotomized-owls) by Heydon Pickering.


#### Installation

Using npm:

```sh
npm i -S jigsass-utils-spacing
```

#### Usage
Import JigSass Utils Spacing into your main scss file near its very end, together with all
other utilities (utilities should always be the last to be imported).

```scss
@import 'path/to/jigsass-utils-spacing/scss/index';
```

Like all other JigSass Utils, JigSass Spacing does not automatically generate any CSS
when imported. You would need to explicitly indicate that each individual spacing
class should actually be generated in each component or object it is used in
(clarification: This will include style declarations inside `.foo` and .`bar`):

```scss
// _c.foo.scss
.foo {
  @include jigsass-util(u-mt, $modifier: 6px); // <-- margin-top: 6px

  ...
}
```

```scss
// _c.bar.scss
.bar {
  @include jigsass-util(u-pb, $modifier: 12px);  // <-- padding-bottom: 12px
  @include jigsass-util(u-m, $modifier: 0, $from: large); // <-- margin: 0 from large bp and on.

  ...
}
```

Doing so helps us a great deal with portability, as no matter where we import component or object
partials, the correct utility classes will be generated. Think of it as a poor man's dependency
management.

Developer communication is also assisted by including "dependencies" wherever they are required,
as anyone going through a partial, can easily understand how it should be marked up with just a
glance.

As far as bloat goes, just don't worry about it - the actual styles will only be generated once,
at the location in the cascade where the Jigsass Spacing partial was imported into the main file.


JigSass Spacing classes are responsive-enabled, using [JigSass MQ](https://txhawks.github.io/jigsass-tools-mq/)
and the breakpoints defined in the [$jigsass-breakpoints](https://txhawks.github.io/jigsass-tools-mq/#variable-jigsass-breakpoints) variable.

Based on the breakpoint arguments passed to `jigsass-util` when including a Spacing class,
responsive modifiers are generated according to the following logic:

```scss
.u-m--<modifier>[-[-from-<breakpoint-name>][-until-<breakpoint-name>][-misc-<breakpoint-name>]]
```

So, assuming the `medium`, `large` and `landscape` breakpoints are defined in `$jigsass-breakpoints`
as `600px`, `1024px` and `(orientation: landscape)` respectively,

```scss
@include jigsass-util(u-m, $modifier: 2);
```
will generate the `.u-m--2` class, which is not limited to any media-query.

```scss
@include jigsass-util(u-p, $modifier: 4, $until: medium);
```

will generate the `.u-p--4--until-medium` class, which will be in effect at
`(max-width: 37.49em)` and will override styles in the default class until that point.

```scss
@include jigsass-util(u-mb, $modifier: 12px, $from: large, $misc: landscape);
```

will generate the `.u-mb--12px--from-large-when-landscape` class, which will go into
effect at `(min-width: 64em) and (orientation: landscape)` and will override styles in the default
class under these  conditions.

## Documentation

The full documentation was generated using mdcss, and is available at 
[https://txhawks.github.io/jigsass-utils-spacing/](https://txhawks.github.io/jigsass-utils-spacing/)

## Contributing

It is a best practice for JigSass modules to *not* automatically generate css on `@import`, but 
rather have the user explicitly enable the generation of specific styles from the module.

Contributions in the form of pull-requests, issues, bug reports, etc. are welcome.
Please feel free to fork, hack or modify JigSass Spacing in any way you see fit.

#### Writing documentation

Good documentation is crucial for usability, scalability and maintainability. When 
contributing, please do make sure that both its Sass functionality (functions, mixins, 
variables and placeholder selectors), as well as the CSS it generates (selectors, 
concepts, usage exmples, etc.) are well documented.

Jigsass Spacing uses Jonathan Neal's [mdcss](//github.com/jonathantneal/mdcss).

When styles and documentation comments are not automatically generated by your module on `@import`,
please use the `sgSrc/sg.scss` file to enable their generation.

In addition, any file in `sgSrc/assets` will be available for use in the style guide.



## File structure
```bash
┬ ./
│
├─┬ scss/ 
│ └─ index.scss # The module's importable file.
│
├─┬ sgSrc/      # Style guide sources
│ │
│ ├── sg.scc    # It is a best practice for JigSass 
│ │             # modules to not automatically generate 
│ │             # css and documentation on `@import.` 
│ │             # Please use this file to enable css
│ │             # and documentation comments) generation.
│ │
│ └── assets/   # Files in `sgSrc/assets` will be 
│               # available for use in the style guide
│
└─┬ docs/      # Documention
  │
  └── styleguide/ # Generated documentation 
                  # of the module's CSS
```

**License:** MIT



[npm-image]: https://badge.fury.io/js/jigsass-utils-spacing.svg
[npm-url]: https://npmjs.org/package/jigsass-utils-spacing

[daviddm-image]: https://david-dm.org/TxHawks/jigsass-utils-spacing.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/TxHawks/jigsass-utils-spacing
