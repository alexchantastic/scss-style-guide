# SCSS Style Guide

This document outlines a guide on the best practices to write semantic, scalable, and modular SCSS/SASS. Note that these guidelines assume the use of [SASS](http://sass-lang.com/), a CSS pre-processor, and more specifically, the SCSS syntax.

The basis for this guide is to outline distinct rules to format and write SCSS, though the principles outlined here can be applied to CSS or SASS syntax as well. Again, the main philosophy is to write SCSS that is as semantic, scalable, and modular as possible. This means that adding to or modifying existing styles should be _extremely_ easy.

This guide borrows concepts from [GitHub's CSS Styleguide](https://github.com/styleguide/css) and [Idiomatic CSS](https://github.com/necolas/idiomatic-css). Feel free to fork this guide as well!

## Table of Contents

* [Code Formatting](#code-formatting)
* [Units](#units)
* [File Structure](#file-structure)
* [Specificity & Naming](#specificity--naming)
* [Ordering](#ordering)

## Code Formatting

### Declaractions

* Use consistent tabs throughout (preferably a tab character with a four space indent)
* A rule declaraction should have a space between the class name and the `{`
* A property declaraction should have a space between the `:` and the value
* A rule declaration should have a space between each selector unless it is a pseudo selector 
* Colors should be specified in hex values unless transparency is required, in which case rgba should be used
* Hex values should be in lowercase and use shorthand if possible
* Comma-separated values should have a space after the `,`
* Multiple comma-separated property values should appear indented on multiple lines with one rule per line
* Decimal values should start with a leading number before the `.`
* Vendor prefixes should be indented so that their values line up to the value that is farthest out
* Use double quotes (`""`) when needed
* Avoid specifying units for zero-values
* Single property declarations should appear on a single line
* Multi property declarations should appear on multiple lines with one rule per line
* Parent selectors should appear on a new line with an empty line separting them from other declarations

### Comments

* Block comments should use the `/* */` syntax
* Single line comments should use the `//` syntax
* Use comment headings and sub-headings appropriately

### Sample

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
	font: {
		family: "Times New Roman", serif;
		size: 20px;
	}
	text-shadow:
		0 1px 2px #ddd,
		0 2px 2px #aaa;
	color: #000;
	content: "Hello world";
	background: rgba(255, 255, 255, 0.15);
	-webkit-box-shadow: 0 0 10px 0 #000;
	   -moz-box-shadow: 0 0 10px 0 #000;
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

* Use `px` for `font-size`
* `line-height` should be unitless (multiplier of `font-size`)

## File Structure
Coming soon!

## Specificity & Naming

### IDs vs Classes

When writing style declarations, always use classes. IDs should be reserved for JS usage. This is to _increase_ the amount of style resuability and _reduce_ the amount of overriding that needs to be done in the CSS codebase.

### Naming Conventions

Rules should be named so that someone looking at just the markup should be able to determine, to a certain degree, what that rule is going to do.

Class names should be all lower case and use `-`s when needed.

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

## Ordering

* Enforeced with [CSScomb](http://csscomb.com/)
* Use nested rules when possible
* Order side values by `top` `right` `bottom` `left`
* Includes first, then mixings, then regular properties

```scss
.my-selector {
	// Includes & Extends
	@include my-mixin;
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
	padding: {
		top: 10px;
		bottom: 10px;
	}
	border: 10px solid #000;
	margin: 10px;

	// Typography
	font: {
		family: "Times New Roman", serif;
		size: 12px;
		style: italic;
		weight: bold;
	}
	color: #000;
	line-height: 1.5;
	text-align: center;
	text-decoration: underline;
	text-transform: uppercase;
	text-shadow: 0 1px 0 #fff;

	// Other
	background: #aaa;
	box-shadow: 0 0 10px #000;
}
```
