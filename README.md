# SCSS Style Guide

This document outlines a guide on the best practices to write semantic, scalable, and modular CSS. Note that these guidelines assume the use of [SASS](http://sass-lang.com/), a CSS pre-processor, and more specifically, the SCSS syntax.

The basis for this guide is to outline distinct rules to format and write SCSS, though the principles outlined here can be applied to CSS or SASS syntax as well. Again, the main philosophy is to write SCSS that is as semantic, scalable, and modular as possible. This means that adding to or modifying existing styles should be _extremely_ easy.

This guide borrows concepts from [GitHub's CSS Styleguide](https://github.com/styleguide/css), [Idiomatic CSS](https://github.com/necolas/idiomatic-css), and [BEM](http://bem.info/method/). Feel free to fork this guide as well!

## Table of Contents

* [Code Formatting](#code-formatting)
* [Units](#units)
* [File Structure](#file-structure)
* [Specificity & Naming](#specificity--naming)
* [Ordering](#ordering)

## Code Formatting

### Declaractions

* Use consistent tabs throughout (preferably a soft tab with a two space indent)
* A rule declaraction should have a space between the class name and the `{`
* A property declaraction should have a space between the `:` and the value
* A rule declaration should have a space between each selector unless it is a pseudo selector 
* Colors should be specified in hex values unless transparency is required, in which case rgba should be used
* Hex values should be in lowercase and use shorthand if possible (ex: `#fff`)
* rgba values should utilize SASS conversion to maintain the hex value if possible (ex: `rgba(#1a74ba, 0.5)`)
* Comma-separated values should have a space after the `,`
* Multiple comma-separated property values should appear indented on multiple lines with one rule per line
* Decimal values should start with a leading number before the `.` (ex: `0.5`)
* Vendor prefixes should be taken care of by [Autoprefixer](https://github.com/ai/autoprefixer)
* If vendor prefixes are necessary, they should be indented so that their values line up to the value that is farthest out
* Use single quotes when needed (ex: `'foo'`)
* Avoid specifying units for zero-values (ex: `margin: 0`)
* Single property declarations should appear on a single line
* Multi property declarations should appear on multiple lines with one rule per line
* Parent selectors should appear on a new line with an empty line separting them from other declarations
* Mixins without passed in variables should omit the `()` (ex: `@include my-mixin`)
* Avoid the use of `!important`

### Comments

* Block comments should use the `/* */` syntax
* Single line comments should use the `//` syntax
* Use comment headings and sub-headings appropriately

### Example

```scss
/* ==================================================
   This is a comment heading
   ================================================== */

/* This is a comment sub-heading
   ================================================== */

/**
  * This is a comment block
  *
  * Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque eu enim leo. 
  * Maecenas odio sem, porttitor at semper eget, vulputate eu ante.
  *
  * Sed rhoncus diam a orci vestibulum molestie. Nulla facilisi. Integer erat nibh, 
  * dictum ac tincidunt eu, mattis eget erat. Nullam pellentesque lorem vel diam 
  * luctus vitae bibendum nisl tristique.
  */

// This is a good example
.foo,
.bar {
	position: absolute;
	display: block;
	font-family: 'Times New Roman', serif;
	font-size: 20px;
	text-shadow: 0 1px 2px #ddd,
		         0 2px 2px #aaa;
	color: #000;
	content: "Hello world";
	background: rgba(255, 255, 255, 0.15);
	box-shadow: 0 0 10px 0 #000;
}

.baz { display: none; }

.foobar .barfoo:first-child {
	color: #fff;

	&:hover { text-decoration: underline; }

	&:active { font-weight: bold; }
}
```

## Units

* Use `rem` for `font-size`
* `line-height` should be unitless (multiplier of `font-size`)

## File Structure

Your file structure can vary, but it is important to try to keep things as organized and consistant as possible since debugging via the browser is much more complex (due to it's compiled nature). Using a source map may help you as well.

Here is an example of a good file structure:

```
css/
	pages/
		_pages.scss
		blog.scss
		home.scss
		login.scss
	_forms.scss
	_global.scss
	_layout.scss
	_mixins.scss
	_reset.scss
	_sprites.scss
	_typography.scss
	_utilities.scss
	_variables.scss
	app.scss
```

`core.scss` will `@import` all of the other SCSS components. Any files in the `pages/` directory are used for page specific styles when absolutely necessary. `_pages.scss` is included in each page specific `.scss` file to give access to app mixins, variables, and utilities.

Partial files should be should be named starting with an `_` and be pluralized (ex: `_buttons.scss`).

Within a component file, the organization should be:

1. Component blocks
2. Component modifiers
3. Elements
4. Element modifiers

## Specificity & Naming

### IDs vs Classes

When writing style declarations, always use classes. IDs should be reserved for JS usage. This is to _increase_ the amount of style resuability and _reduce_ the amount of overriding that needs to be done in the CSS codebase.

If there is an instance where a specific class is needed for a specific element, try to name the class in a semantic manner. For example, `.twitter-box` or `.facebook-box`.

### Naming Conventions

Rules should be named so that someone looking at just the markup should be able to determine, to a certain degree, what that rule is going to do.

Class names should use all lower case and adhere to a BEM-like style.

```scss
.media {
	...
}

.media-object {
	...
}

.media-object-reversed {
	...
}

.media-body {
	...
}
```

```html
<div class="media">
	<div class="media-object media-object-reversed"></div>
	<div class="media-body"></div>
</div>
```

As you can see, the class names are broken down into first a block (`.media`), as refered to as a component. Then, into elements (`.media-object` and `.media-body`). And finally into modifiers (`.media-object-reversed`).

Your naming scheme for blocks, elements, and modifiers should be clear so that there is no confusion as to wheter a prefix is an elment or a modifier. This scheme prevents any unnecessary nesting so that our SCSS maintains its flexibility.

### Nesting

While nesting is a great feature of SASS, it can easily make things much more complex and hard to digest. Nesting should be limited to 4 levels or less.

### Extending

Again, while extending selectors (`@extend`) is a great feature of SASS, it can easily make this much more complex and hard to digest. Extends should be used sparingly and only when they can be easily grouped together. More often than not, it would make more sense to pull something out into a mixin rather than use an extend.

### Media Queries

SASS allows media queries to be nested inside of a selector. This can make our code very easy to digest when working in responsive styles.

```scss
.my-selector {
	height: 100px;
	backgroud-color: #000;

	@media (min-width: 1024px) {
		height: 200px;
		background-color: #fff;
	}
}
```

However, when working with specific breakpoint classes, then it is advisable to use the regular approach where selectors are nested within the `@media` block.

### Rule & Property Specificity

Rules and properties should only be as specific as they need to be. Rules should not be chained or nested just for the sake of being chained or nested. Property values should not be defined for the sake of using shorthand.

```scss
// Bad :(
.body .content .my-selector {
	margin: 0 0 20px 0;
	padding: 10px 0 0 0;
}

// Good :)
.my-selector {
	margin-bottom: 20px;
	padding-top: 10px;
}
```

Additional, excessive use of child selectors should be avoided to maintain flexibility.

```scss
// Bad :(
ul.tabs > li.tab {
	...
}

// Good :)
.tabs {
	...
}

.tab {
	...
}
```

## Ordering

* Try not to use nested rules
* Order side values by `top` `right` `bottom` `left`
* `@extend`s first, then regular properties
* Mixins (`@include`) should go with the relevant section, but if it is a complex mixin, put it at the end

```scss
.my-selector {
	@extend .my-other-selector;

	// Positioning
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	z-index: 10;

	// Box Model
	display: block;
	overflow: hidden;
	box-sizing: border-box;
	width: 100px;
	height: 100px;
	padding-top: 10px;
	padding-bottom: 10px;
	border: 10px solid #000;
	@include border-radius(10px);
	margin: 10px;

	// Typography
	font-family: 'Times New Roman', serif;
	font-size: 1rem;
	font-style: italic;
	font-weight: bold;
	color: #000;
	line-height: 1.5;
	text-align: center;
	text-decoration: underline;
	text-transform: uppercase;
	text-shadow: 0 1px 0 #fff;

	// Other
	background: #aaa;
	box-shadow: 0 0 10px #000;

	@include my-mixin;
}
```
