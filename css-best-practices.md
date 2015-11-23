# CSS Coding Guidelines

If you want our scss-lint.yml then you can install [make-up](https://github.com/holidayextras/make-up) which has it packaged inside.

Shortbreaks uses a strict subset of [SASS](http://sass-lang.com/) for style generation. This subset includes variables and mixins, but nothing else (no nesting, etc.).

Shortbreak's use the [BEM](http://bem.info/) syntax for naming conventions and creating morem odular code.
To read more about BEM, please see this [brilliant article](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) by Harry Roberts

**Table of contents**

* [JavaScript](#javascript)
* [BEM](#BEM)
  * [BEM Layout](#bemLayout)
  * [blockName](#blockName)
  * [blockName\_\_elementName](#blockName__elementName)
  * [blockName--modifierName](#blockName--modifierName)
  * [blockName\_\_elementName--modifierName](#blockName__elementName--modifierName)
* [Variables](#variables)
  * [colors](#colors)
* [SASS](#sass)
	* [mixins](#mixins)
* [Formatting](#formatting)
  * [Spacing](#spacing)
  * [Quotes](#quotes)
  * [Specificity](#specificity)


<a name="javascript"></a>
## JavaScript

syntax: `js-<targetName>`

JavaScript-specific classes reduce the risk that changing the structure or theme of components will inadvertently affect any required JavaScript behaviour and complex functionality. It is not neccesarry to use them in every case, just think of them as a tool in your utility belt. If you are creating a class, which you dont intend to use for styling, but instead only as a selector in JavaScript, you should probably be adding the `js-` prefix. In practice this looks like this:

```html
<a href="/login" class="btn btn-primary js-login"></a>
```

**Again, JavaScript-specific classes should not, under any circumstances, be styled.**

<a name="BEM"></a>
## BEM

Syntax: `<block>[__element|--modifier]`

For the best understanding of BEM, either see the [Bem website here]() or read this article about [getting your head around BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

<a name="BEM layout"></a>
### bemLayout

Blocks, elements & modifiers should be linked together using SASS's parent selecter symbol ```&```

**Right:**
```csss
nav[role=navigation] {
}
.nav {
    &__list {
      &__item {
      }
    }
  &__link {
    &--active {
    }
  }
}
```

**Wrong:***
```css
.block {
}
.block__element {
}
.block--modifier {
}

```

<a name="blockName"></a>
### blockName

You can think of block as custom elements that enclose specific semantics, styling, and behaviour.

The block's name must be written in camel case.

```css
.myBlock { /* ... */ }
```

```html
<article class="myBlock">
  ...
</article>
```

<a name="blockName__elementName"></a>
### blockName__elementName

A element represents a descendent of the block that helps form the block as a whole.

```css
/* Core block styles */
.siteSearch { /* ... */ }
/* Element of block styles */
.siteSearch__field { /* ... */ }
```

```html
<div class="siteSearch">
	<input class="siteSearch__field"></input>
</div>
```
<a name="blockName--modifierName"></a>
### blockName--modifierName

A modifier represents a different state or version of the block.

```css
/* Core block styles */
.siteSearch { /* ... */ }
/* Element of block styles */
.siteSearch__field { /* ... */ }
/* Extra styles for a serperate version of .siteSearch */
.siteSearch--big { /* ... */ }
```

```html
<div class="siteSearch siteSearch--big">
	<input class="siteSearch__field"></input>
</div>
```

<a name="blockName__elementName--modifierName"></a>
### blockName__elementName--modifierName

Elements and modifiers can be chained together like so

```css
/* */
.siteSearch__field--long{ /* ... */ }
```

```html
<div class="siteSearch siteSearch--big">
	<input class="siteSearch__field siteSearch__field--long"></input>
</div>
```

<a name="variables"></a>
## Variables

Syntax: `<property>__<value>[--blockName]`

Variable names in our CSS are also strictly structured. This syntax provides strong associations between property, use, and component.

The following variable defintion is a color property, with the value grayLight, for use with the highlightMenu component.

```CSS
@color__grayLight--highlightMenu: rgb(51, 51, 50);
```

<a name="colors"></a>
### Colors

When implementing feature styles, you should only be using color variables provided by colors.less.

When adding a color variable to colors.less, using RGB and RGBA color units are preferred over hex, named, HSL, or HSLA values.

**Right:**
```css
rgb(50, 50, 50);
rgba(50, 50, 50, 0.2);
```

**Wrong:**
```css
#FFF;
#FFFFFF;
white;
hsl(120, 100%, 50%);
hsla(120, 100%, 50%, 1);
```

<a name="SASS"></a>
## SASS

Where possible try to avoid nesting in SASS. At very most aim for no more than three levels.

<a name="mixins"></a>
### mixins

mixin syntax: `m-<propertyName>`

An example of a border radius mixin:

```css
.m-borderRadius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
              border-radius: @radius;
}
```

<a name="formatting"></a>
## Formatting

The following are some high level page formatting style rules.

<a name="spacing"></a>
### Spacing

CSS rules should be comma seperated but live on new lines.

**Right:**
```css
.content,
.content-edit {
  ...
}
```

**Wrong:**
```css
.content, .content-edit {
  ...
}
.content, .content-edit { ... }
```

CSS blocks should be seperated by a single new line. not two. not 0.

**Right:**
```css
.content {
  ...
}
.content-edit {
  ...
}
```

**Wrong:**
```css
.content {
  ...
}

.content-edit {
  ...
}
```

A space should be before ``` { ``` in rule declarations, also line break after ``` { ``` and before ``` } ```

**Right:**
```css
.content {
  ...
}
```

**Wrong:**
```css
.content{
  margin:10px
}
.content{margin:10px}
```

A space should be after, and not before ``` : ``` in rule declarations

**Right:**
```css
.content {
  margin: 10px;
}
```

**Wrong:**
```css
.content{
  margin: 10px
}
.content{
  margin:10px
}
```


<a name="quotes"></a>
### Quotes

Quotes are optional in CSS and LESS. We use double quotes as it is visually clearer that the string is not a selector or a style property.

**Right:**
```css
background-image: url("/img/you.jpg");
font-family: "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial;
```

**Wrong:**
```css
background-image: url(/img/you.jpg);
font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
```

<a name="specificity"></a>
### Specificity

Although in the name (cascading style sheets) cascading can introduce unnecessary performance overhead for applying styles. Take the following example:

```css
ul.user-list li span a:hover { color: red; }
```

Styles are resolved during the renderer's layout pass. The selectors are resolved right to left, exiting when it has been detected the selector does not match. Therefore, in this example every a tag has to be inspected to see if it resides inside a span and a list. As you can imagine this requires a lot of DOM walking and and for large documents can cause a significant increase in the layout time. For further reading checkout: https://developers.google.com/speed/docs/best-practices/rendering#UseEfficientCSSSelectors

If we know we want to give all `a` elements inside the `.user-list` red on hover we can simplify this style to:

```css
.user-list > a:hover {
  color: red;
}
```

If we want to only style specific `a` elements inside `.user-list` we can give them a specific class:

```css
.user-list > .link-primary:hover {
  color: red;
}
```

Any selector that once compiled has more than 2 levels of nesting will fail a code review, for example:

**Right:**

```css
.element .element-inner {
	// stuff
}
```

```css
.element {
	&-inner {
		// stuff
	}
}
```

**Wrong:***

```css
.element .element-inner a {
	// stuff
}
```

```css
.element {
	&-inner {
		a {
			// stuff
		}
	}
}
```

Credit goes to [Medium](https://medium.com) and [Fat](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06) as this has been ~~stolen from~~ built ontop of there code guidlines.