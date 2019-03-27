# (S)CSS Style Guide

<p align="center">
  <img src="img/logo.png"/>
</p>

Mono uses [Sass](http://sass-lang.com/) for style generation.

Other Style Guides

  - [VueJS](https://github.com/monosolutions/vuejs-style-guide)
  - [JavaScript](https://github.com/monosolutions/javascript-style-guide)

**Table of contents**

1. [SCSS](#scss)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [ID Selectors](#idSelectors)
    - [Border](#border)
    - [Ordering](#ordering)
    - [Mixins](#mixins)
    - [Extend directive](#extend)
    - [Nested selectors](#nested)
1. [Linting](#linting)

**[⬆ back to top](#table-of-contents)**
<a name="scss"></a>
## SCSS
<a name="formatting"></a>
### Formatting

* Use soft tabs (4 spaces) for indentation ( stylelint ref.: [indentation](https://stylelint.io/user-guide/rules/indentation/) ).
* Prefer camelCasing over dashes in class names.
* Do not use ID selectors.
* When using multiple selectors in a rule declaration, give each selector its own line ( stylelint ref.: [selector-list-comma-newline-after](https://stylelint.io/user-guide/rules/selector-list-comma-newline-after/) ).
* Put a space before the opening brace `{` in rule declarations ( stylelint ref.: [block-opening-brace-space-before](https://stylelint.io/user-guide/rules/block-opening-brace-space-before/) ).
* In properties, put a space after, but not before, the `:` character ( stylelint ref.: [declaration-colon-space-after](https://stylelint.io/user-guide/rules/declaration-colon-space-after/) and  [declaration-colon-space-before](https://stylelint.io/user-guide/rules/declaration-colon-space-before/) ).
* Put closing braces `}` of rule declarations on a new line ( stylelint ref.: [block-closing-brace-newline-before](https://stylelint.io/user-guide/rules/block-closing-brace-newline-before/) ).
* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
    // ...
}
```

**Good**

```css
.avatar {
    border-radius: 50%;
    border: 2px solid white;
}

.one,
.selector,
.perLine {
    // ...
}
```
<a name="comments"></a>
### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

<a name="idSelectors"></a>
### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

<a name="border"></a>
### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
    border: none;
}
```

**Good**

```css
.foo {
    border: 0;
}
```
<a name="ordering"></a>
### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
        background: green;
        font-weight: bold;
        // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
        background: green;
        font-weight: bold;
        @include transition(background 0.5s ease);
        // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
        background: green;
        font-weight: bold;
        @include transition(background 0.5s ease);

        .icon {
            margin-right: 10px;
        }
    }
    ```

<a name="mixins"></a>
### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

<a name="extend"></a>
### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

<a name="nested"></a>
### Nested selectors

**Do not nest selectors more than four levels deep!**

```scss
.pageContainer {
    .content {
        .profile {
    	    .user {
      	        // STOP!
      	    }
        }
     }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

<a name="linting"></a>
## Linting

We use [stylelint](https://stylelint.io/) for linting the scss files. The configuration file for the linting can be found in [tool-ii](https://github.com/monosolutions/tool-ii/blob/master/.stylelintrc) for example. There's an autofix option that covers most of the things the linting checks for (like missing/too many new lines/spaces).

We also use a stylelint plugin to define the order of the properties within each block ([link](https://github.com/hudochenkov/stylelint-order)). This is there to help the developer with writing better scss - by having an organised way to order the properties it is much faster to understand the layout of the page and fix/add more style to an element. A reference to the order can be found in [tool-ii](https://github.com/monosolutions/tool-ii/blob/master/.stylelintrc) for example.

---

**Disclaimer**

This has been highly influenced by the [Airbnb CSS / Sass Styleguide](https://github.com/airbnb/css)