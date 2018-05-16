# SCSS Style Guide

This document outlines best practices on writing semantic, scalable, and modular CSS. Note that these guidelines assume the use of [SASS](http://sass-lang.com/), a CSS pre-processor, and more specifically, the SCSS syntax. The basis for this guide is to outline distinct rules to format and write SCSS, though the principles outlined here can be applied to CSS or SASS syntax as well.

This guide borrows concepts from [Code Guide by @mdo](http://codeguide.co/), [Idiomatic CSS](https://github.com/necolas/idiomatic-css), and [BEM](http://bem.info/method/).

## Table of Contents

* [Code Formatting](#code-formatting)
* [Specificity & Naming](#specificity--naming)
* [Units](#units)
* [Ordering](#ordering)
* [Linting](#linting)

## Code Formatting

### Declaractions

* Use soft tabs with a two space indent
* A selector should have a single space between the name and the `{`
* A property should have a single space between the `:` and the value
* There should be only one property per line
* Unrelated or non-sequenced selectors should have a single new line between them
* Colors should be specified in hex or rgba values
* Hex values should be in lowercase and use shorthand if possible (e.g., `#fff`)
* rgba values should utilize SASS conversion to maintain the hex value (e.g., `rgba(#fff, 0.5)`)
* Comma-separated values should have a single space after the `,` if they appear on a single line
* Multiple comma-separated property values should appear indented on multiple lines with one declaration per line
* Decimal values should start with a leading number before the `.` (e.g., `0.5`)
* Psuedo selectors and elements should be denoted using a single `:` (e.g., `:hover`, `:before`)
* Vendor prefixes should be taken care of by an [autoprefixer](https://github.com/postcss/autoprefixer)
* Use single quotes (e.g, `'foo'`)
* Do not specify units for zero-values (e.g, `margin: 0`)
* Single property declarations should appear on a single line
* Multi property declarations should appear on multiple lines with one rule per line
* Mixins without properties should omit the `()` (e.g., `@include my-mixin`)
* Avoid the use of SASS color functions (e.g., `lighten(#000)`)
* Avoid the use of `!important`

### Comments

* Use the SCSS comment syntax (`//`) for all comments
* Use comment headings appropriately

### Example

```scss
//
// This is a comment heading
//

//
// This is a comment block heading
//
// Iaculis velit a laoreet consectetur mollis dignissim diam fringilla,
// commodo vulputate turpis dis praesent lacus lorem, justo accumsan
// platea scelerisque pharetra viverra id.

// This is a good example
.foo,
.bar {
  position: absolute;
  display: block;
  font-family: 'Times New Roman', serif; // This is another good example
  font-size: 20px;
  color: #000;
  text-shadow:
    0 1px 2px #ddd,
    0 2px 2px #aaa;
  content: 'Hello world';
  background: rgba(#fff, 0.15);
  box-shadow: 0 0 10px 0 #000;
}

.foobar {
  color: #fff;

  &:hover { text-decoration: underline; }

  &:active { font-weight: bold; }
}

.biz { font-size: 16px; }
.baz { font-size: 24px; }
```

## Units

* Use `px` for things that should scale in a fixed manner
* Use `rem` for things that should scale with the root `font-size`
* Use `em` for things that should scale with the element `font-size`
* Use `%` for things that should scale with the parent element
* Use `vw` and `vh` for things that should scale with the viewport
* `line-height` should be a unitless multiplier of `font-size`

## Specificity & Naming

### Rule Specificity

Rules and properties should only be as specific as they need to be. Ideally, your selectors should maintain a flat structure.

```scss
// Bad 🙁
.grandparent .parent .my-selector {
  ...
}

// Good 😀
.my-selector {
  ...
}
```

### Selectors

* Always use class selectors
* Do not use ID selectors
* Do not use type selectors
* Do not mix class selectors with ID or type selectors
* Use attribute selectors wisely

Adhering to these guidelines _increases_ the amount of style resuability and _reduces_ the amount of overriding that needs to be done in the CSS codebase.

```scss
// Bad 🙁
div {
  ...
}

div.my-selector {
  ...
}

#my-selector {
  ...
}

// Good 😀
.my-selector {
  ...
}
```

### Naming Conventions

Rules should be named semantically so that someone looking at just the markup should be able to determine what that rule is going to do.

* Use BEM style for components
* Use kebab-case for multiple words (e.g., `.my-block`)
* Use two underscores (`__`) to separate blocks from elements (e.g., `.my-block__my-element`)
* Use two dashes (`--`) to separate modifiers from blocks or elements (e.g., `.my-block--my-modifier` or `.my-block__my-element--my-modifier`)

```scss
// Component Classes
.media {
  ...

  &__object {
    ...

    &--reversed { ... }
  }

  &__body {
    ...
  }
}

// Utility Classes
.text-sm { ... }
.text-md { ... }
.text-lg { ... }
```

Class names are broken down first into a block (`.media`). Then, into elements (`.media__object` and `.media__body`). And finally into modifiers (`.media__object--reversed`).

### Nesting

While nesting is a great feature of SASS, it can easily make things much more complex and hard to digest. Nesting should be limited to 3 levels or less.

### Extending

While extending selectors (`@extend`) is a great feature of SASS, it can easily make the output CSS much more complex and hard to digest so it is best to avoid using them. More often than not, it would make more sense to pull something out into a mixin rather than use an extend.

### Shorthand

Limit the use of shorthand declarations if you do not need to explicitly set additional declarations.

```scss
// Bad 🙁
.my-selector {
  margin: 10px 0 0 0;
  border-radius: 4px 0 0 0;
}

// Good 😀
.my-selector {
  margin-top: 10px;
  border-top-left-radius: 4px;
}
```

### Media Queries

SASS allows media queries to be nested inside of a selector. This can make our code very easy to digest when working in responsive styles.

```scss
.my-selector {
  ...

  @media (min-width: 1024px) {
    ...
  }
}
```

### Variables

SASS variables should be used wherever a property value may need to be repeated across the codebase (e.g., color values, breakpoints).

* Use kebab-case (e.g., `$color-white`)
* Use semantic naming conventions

```scss
// Bad 🙁
$alpha: #000;
$beta: #fff;

// Good 😀
$color-black: #000;
$color-white: #fff;
```

## Ordering

* Order side values by `top`, `right`, `bottom`, `left`
* Declare mixins first, then regular properties
* Psuedo selectors should come before other selectors

```scss
.my-block {
  @include my-mixin;

  // Positioning
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 10;

  // Box Model
  box-sizing: border-box;
  display: block;
  width: 100px;
  height: 100px;
  padding-top: 10px;
  padding-bottom: 10px;
  margin: 10px;
  overflow: hidden;

  // Typography
  font-family: 'Times New Roman', serif;
  font-size: 1rem;
  font-style: italic;
  font-weight: bold;
  line-height: 1.5;
  color: #000;
  text-align: center;
  text-decoration: underline;
  text-shadow: 0 1px 0 #fff;
  text-transform: uppercase;

  // Accessibility & Interaction
  cursor: pointer;

  // Background & Borders
  background: #aaa;
  border: 10px solid #000;
  box-shadow: 0 0 10px #000;

  // Transitions & Animations
  transition: all 100ms ease;

  &:before { ... }

  &:hover { ... }

  &--my-modifier { ... }

  &__my-element { ... }
}
```

## Linting

It is best to leverage a linter like [stylelint](https://github.com/stylelint/stylelint) to make it easy to adhere to the following guidelines. You can install this repository through your package manager and point to it in your own stylelint config file. Alternatively, you can copy the stylelint config file ([`.stylelintrc`](https://github.com/alexchantastic/scss-style-guide/blob/master/.stylelintrc)) included in this repository into your project.

### Installation

#### Yarn

```
yarn add stylelint scss-style-guide --dev
```

#### npm

```
npm install stylelint scss-style-guide --save-dev
```

#### `.stylelintrc`

```
{
  "extends": [
    "scss-style-guide"
  ]
}
```
