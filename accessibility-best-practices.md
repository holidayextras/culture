# Best Practices for Accessible Web Content

**Table of contents**

* [Text alternatives for images](#text-alternatives)
* [Labels on form fields](#labels)
* [Headings](#headings)
* [Links](#links)
* [Title attributes](#title-attributes)
* [Keyboard accessibility](#keyboard)
* [Hiding content in an accessible way](#hidden)

<a name="text-alternatives"></a>
## Text alternatives for images

A text alternative is not always necessary (see below) but every `img` element must still have an `alt` attribute, regardless of whether or not it holds a value. Without an `alt` attribute, screen readers and other assistive technology will often default to announcing the file name, which does not provide a good user experience.

### Decorative images
If an image is purely decorative or does not contain any more information than is already available in text, the `alt` attribute should be empty. Images with empty `alt` attributes are ignored by assistive technology, avoiding unnecessary noise.

In the following example, an image of a telephone is displayed next to a phone number. The heading already provides context and meaning for the number so there is no value in the image being announced as well. The image should therefore have an empty `alt` attribute so that it is ignored completely.

```html
<h2>Call our free UK Helpline</h2>
<img src="/images/telephone.png" alt="">
<p>0800 093 5478</p>
```

### Images in links
If an image is used as a link instead of text, the `alt` attribute on the `img` element should indicate the purpose of the link. Follow the principles described in [Links](#links).

<a name="labels"></a>
## Labels on form fields

Every form field (apart from buttons) must have a `label` that describes its purpose.

Each `label` should be explicitly paired with its field by a `for` attribute on the `label` equal to the `id` on the field. Not only does this allow screen readers and other assistive technology to announce the correct label when the field receives focus, it also increases the size of the clickable area for other users.

In the example below, the "Flying from?" label has as `for="location"` attribute which explicitly pairs it with the `select` control that has a corresponding `id="location"` attribute.

```html
<label for="location">Flying from?</label>
<select name="location" id="location">
    <option value="ABZ">Aberdeen</option>
    <option value="BFS">Belfast International</option>
    ...
</select>
```

Placeholder text (a default vaule in the field) should **not** be used instead of a separate `label` element. Placeholders are cleared on focus and often not announced at all by screen readers or other assistive technology.

### Hidden labels

In some cases, it might be appropriate to visually hide a label. For example, where separate fields are provided for the day, month and year of a date, it is often unnecessary to display the label for each. In this situation, it is acceptable to hide the labels with CSS as long as they remain accessible to assistive technology in the HTML. See the section on [Hiding content in an accessible way](#hidden) for an example of this.

```html
<fieldset>
    <legend>Date of birth</legend>
    <label for="day" class="hidden">Day</label>
    <input id="day" type="text">
    <label for="month" class="hidden">Month</label>
    <select id="month">
        <option value="jan">January</option>
        <option value="feb">February</option>
        ...
    </select>
    <label for="year" class="hidden">Year</label>
    <input id="year" type="text">
</fieldset>
```

### Labels for checkboxes and radio buttons
As well as being explicitly associated with `for` and `id` attributes, labels for checkboxes and radio buttons should appear *after* the control in the markup and be displayed *to the right* of the control on screen. This convention ensures compatibility with the widest range of assistive technologies, and is also consisitent with UI patterns established by Microsoft and Apple.

<a name="headings"></a>
## Headings
HTML heading elements (`h1` to `h6`) must be used to denote headings so that the structure of the page is clear.

It not sufficient to simply make something *look* like a heading by, for example, increasing its font size or changing its colour; such changes are purely visual and do not convey any semantic meaning that can be interpreted by assistive technology. Equally, heading elements should not be used to apply styling to content that is not a heading.

Headings should follow a hierarchical structure and levels should not be skipped. For example, an `h2` should not be followed by an `h4` without an intervening `h3`.

```html
<!-- Anti-pattern, do not copy -->
<h2>Our privacy policy</h2>
<h4>What personal information do we ask for?</h4>
<p>When you have searched for prices and availability and are ready to confirm your booking, we will ask for your name, your postal address,your fax number and e-mail address.
...
</p>
```

<a name="links"></a>
## Links

Generic link phrases such as "click here", "more information" or "this page" should not be used because their purpose is unknown without considering the wider context (e.g. without reading the sentence):

```html
<!-- Anti-pattern, do not copy -->
<a href="">Click here</a> to book airport parking now or see <a href="">more information</a> about upgrades.
```

Instead, link phrases should describe their own purpose:

```html
<a href="">Book airport parking now</a> or see more <a href="">information about upgrades</a>.
```

The same link phrase should not be used more than once on the same page, unless those links point to the same place. Equally, avoid using different phrases to link to the same place wherever possible.

<a name="title-attributes"></a>
## Title attributes
`title` attributes should not be used to provide important information because they are often ignored by screen readers and other assistive technology. They are also only displayed very briefly to mouse users and not at all on touch devices.

<a name="keyboard"></a>
## Keyboard accessibility


<a name="landmarks"></a>
## Landmark roles


<a name="hidden"></a>
## Hiding content in an accessible way
The `display: none` and `visibility: hidden` properties in CSS both hide content in a way that makes it inaccessible to screen readers and other assistive technology. They should therefore generally only be used if the intention is to hide the content from *all* users.

The most reliable way to hide content visually while keeping it accessible to assistive technology behind the scenes is to position it off the viewport.

```css
.hidden { 
    position: absolute;
    left: -10000px;
    top: auto;
    width: 1px;
    height: 1px;
    overflow: hidden;
    }
```

If hidden content can receive keyboard focus, it should be made visible when focused. This avoids the confusing situation where keyboard focus disappears and then reappears when tabbing through a page.