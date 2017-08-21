# Best Practices for Accessible Web Content

**Table of contents**

* [Testing](#testing)
* [Text alternatives for images](#text-alternatives)
* [Labels on form fields](#labels)
* [Headings](#headings)
* [Links](#links)
* [Title attributes](#title-attributes)
* [Keyboard accessibility](#keyboard)
* [Tables](#tables)
* [Hiding content in an accessible way](#hidden)
* [Announcing changes](#live)

## Need help with accessibility?
Have an accessibility question? Want us to check your work for accessibility issues? Speak to any of the Accessibility stretch holders who can be found in the [#stretch-accessibility](https://holidayextras.slack.com/messages/stretch-accessibility) channel on Slack.

<a name="testing"></a>
## Testing

### Using a keyboard
One of the simplest and most powerful ways to test web content for accessibility is to navigate using a keyboard, without using a mouse or touchpad. Many groups of users rely on a keyboard to navigate and several forms of assistive technology (e.g. keyboard emulators, toggle switches, voice control) work on the same principles.

### Using a screenreader
VoiceOver is a screen reader that is pre-installed on Mac computers, iPhones, iPads and iPod touches. It is a popular solution, particularly with Apple users, but is not the most widely used across all platforms. Testing with one screen reader is much like testing with one browser in that will give some insight into the likely problems but will not be fully representitive.

For more information on the limitations of testing with VoiceOver see WebAIM's article on the [three things you should know before using VoiceOver for testing](http://webaim.org/blog/three-things-voiceover/) and PowerMapper's latest [assistive technology compatibility tests](http://www.powermapper.com/tests/).

WebAIM provides a list of [keyboard shortcuts for controlling VoiceOver](http://webaim.org/articles/voiceover/).

### Other tools

Tool | Description
:----|:----
[aXe for Chrome](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd) | Adds the ability to analyse a web page for accessibility problems. Appears as a separate tab in Chrome developer tools.
[tota11y](https://khan.github.io/tota11y/) | A JavaScript bookmarklet that annotates a web page to highlight accessibility features and problems.
[Chrome Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb?hl=en) | An extension from Google's accessibility team that adds an Accessibility audit and an Accessibility sidebar pane in the Elements tab in Chrome.
[WAVE Chrome Extension](http://wave.webaim.org/extension/) | An evaluation tool developed by WebAIM that provides visual feedback about the accessibility of web content by injecting icons and indicators into the page.
[Colour Contrast Analyser](https://www.paciellogroup.com/resources/contrastanalyser/) | Available for Mac and Windows, provides a pass/fail assessment against WCAG 2.0 colour contrast success criteria as well as a simulation of certain visual conditions.

### Beware automated testing
While tools and automation can detect rudimentary accessibility problems, many more issues can only be identified by a human tester. For example, tools can identify images without `alt` attributes but cannot judge whether the text alternatives that have been provided are appropriate.

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

Placeholder text (a default value in the field) should **not** be used instead of a separate `label` element. Placeholders are cleared on focus and often not announced at all by screen readers or other assistive technology.

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

### Identifying mandatory fields
A mandatory field should be identified in its label, in a way that doesn't rely on vision, so that it can be identified by both signed and non-sighted users. This could be an asterisk (*) or simply say "required". Where an asterisk or other symbol is used, this should be explained at the top of the form (e.g. "fields marked with * are required") and an `abbr` should be included on the symbol itself:

```html
<label>Email address <abbr title="required field">*</abbr></label>
```

Mandatory fields can also be identified programmatically using ARIA's `aria-required="true"` attribute and HTML5's `required` attribute.

Where all fields in a form are mandatory, it is acceptable to simply say "all fields are mandatory", but probably easier (technically) to adopt the same approach throughout and mark all individual fields.

### Form validation
Invalid fields can be identified programmatically using ARIA's `aria-invalid="true"` attribute. Note that `aria-invalid` should not be set to `true` before validation has taken place.

```html
<input id="name" type="text" aria-invalid="true">
```

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

Note that there are situations in which lower level headings appear before the `h1` due to page layout. This is fine as long as they themselves follow a logical hierarchy.

<a name="links"></a>
## Links

Generic link phrases such as "click here", "more information" or "this page" should not be used because their purpose is unknown without considering the wider context (e.g. without reading the sentence). This makes it difficult for people to navigate and also has a negative impact on SEO.

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

`title` attributes should also not simply repeat information that is already provided on the page. This is redundant and annoying for anyone who *can* access it.

```html
<!-- Anti-pattern, do not copy -->
<a href="" title="Book airport parking now">Book airport parking now</a>
```

<a name="keyboard"></a>
## Keyboard accessibility

### Focus visibility
Keyboard focus should be visible at all times so that a sighted keyboard user can see which element of the page they are interacting with. By default, browsers present the `:focus` state with a dotted `outline`. The styling of the `outline` can be modified or replaced, as long as it remains visually obvious.

<a name="landmarks"></a>
### Landmark roles
Landmark roles are a way of extending the native semantics of HTML elements in order to better describe the structure of a page. They make it easier fo assistive technology users to understand and navigate content.

For a demonstration of landmark roles in action, watch the video on [How ARIA landmark roles help screen reader users](https://www.youtube.com/watch?v=IhWMou12_Vk).

The following roles are the most useful:

Role | Description
:----|:----
`role="banner"` | The main header of a page, often applied to the `header` element. Must only be used once per page.
`role="main"` | The main content of a document, often applied to a containing `div` or `main` element. Must only be used once per page.
`role="navigation"` | A list of links serving as a navigation menu, normally used on a `nav` element. There are often several instances of this landmark role on a page and therefore labels should be used to differentiate them (see below).
`role="contentinfo"` | Information about the page including contact information and legal notices, usually applied to a `footer` element. Must only be used once per page.

Note that the need for landmark roles will diminish as support for HTML5 elements improves but for the time being they offer an easy and unobtrusive way to increase accessibility.

#### Providing labels for landmarks
When more than one landmark with the same role is used on the same page, a descriptive label should be provided using either `aria-label` or `aria-labelledby` so that the user can identify the purpose of each landmark.

```html
<!-- Providing a label with the aria-label attribute -->
<nav role="navigation" aria-label="Products and services">
    ...
</nav>
```

```html
<!-- Providing a label with the aria-labelledby attribute -->
<nav role="navigation" aria-labelledby="heading">
    <h2 id="heading">Products and services</h2>
    ...
</nav>
```
<a name="tabindex"></a>
### Tabindex
In general, elements should be left in the default tab order and `tabindex` attributes should not be used to change that order. However, `tabindex` can be used to add or remove elements from the tab order completely.

* Set `tabindex="-1"` to remove an element from the tab cycle but keep it focusable with mouse or JavaScript.
* Set `tabindex="0"` to make an element (such as a `div` or a `span`) receive focus in the tab cycle.
* When an element is given focus in JavaScript rather than as a result of the user tabbing to it, visual focus styling must still be applied.

<a name="modal"></a>
### Keyboard accessibility in modal dialogs
A modal dialog is a dialog that forces the user to interact with it, blocking the rest of the application. They are often used to prompt for a 'yes' or 'no' response.

* Focus should be given to the first focusable element, the primary action inside the dialog or the dialog container when it is opened.
* Focus should be trapped inside the dialog, i.e. it should not be possible for the user to tab onto elements outside of the dialog while it is open.
* When the dialog is closed, keyboard focus should be returned to its original position, or to a sensible point of continuation.
* Pressing the escape key on the keyboard should close the dialog.
* The dialog container should have a `role="dialog"` attribute and be labelled with either `aria-label` or `aria-labelledby`.

```html
<div role="dialog" aria-labelledby="heading">
    <h2 id="heading">For £40.00 more, add a steak dinner to your booking</h2>
    <p>Grab yourself a delicious steak and a glass of wine at the Courtyard's popular Casterbridge Grill restaurant.</p>
    <a href="">No thanks</a>
    <a href="">Yes please</a>
</div>
```

<a name="tables"></a>
## Tables

* `table` elements must be use to present tabular data. Don't use `div`s or other elements to simulate a tabular layout.
* `table` elements must not be used purely to control page layout.
* Row and column headings must be in `th` elements with `scope="row"` or `scope="col"` attributes.

```html
<table>
    <thead>
        <th scope="col">Car Park</th>
        <th scope="col">Distance</th>
        <th scope="col">Transfers</th>
        <th scope="col">Rating</th>
        <th scope="col">Price</th>
    </thead>
    <tr>
        <td>Purple Parking - Flexible</td>
        <td>2.4 miles</td>
        <td>Run every 20 minutes, take 15 minutes and are included in the price</td>
        <td>86%</td>
        <td>£4.00 per day</td>
    </tr>
    <tr>
        <td>Long Stay South</td>
        <td>On-airport</td>
        <td>Run every 10 minutes, take 5 minutes and are included in the price</td>
        <td>90%</td>
        <td>£4.63 per day</td>
    </tr>
</table>
```


<a name="hidden"></a>
## Providing hidden content for assistive technology
The `display: none` and `visibility: hidden` properties in CSS both hide content in a way that makes it inaccessible to screen readers and other assistive technology. They should therefore generally only be used if the intention is to hide the content from *all* users.

The most reliable way to hide content visually while keeping it accessible to assistive technology behind the scenes is to position it off the viewport.

```css
.hidden {
    position: absolute;
    left: -10000px;
    top: auto;
    width: 1px; /* VoiceOver fails to announce elements with 0 width */
    height: 1px; /* VoiceOver fails to announce elements with 0 height */
    overflow: hidden;
    }
```

If hidden content can receive keyboard focus, it should be made visible when focused. This avoids the confusing situation where keyboard focus disappears and then reappears when tabbing through a page.

## Hiding content from assistive technology
The `aria-hidden="true"` and `role="presentation"` attributes can be used to exclude elements from the Accessibility API, hiding them from assistive technology while keeping them visible on the page. This should only be done with the aim of improving the experience for assistive technology users (e.g. to remove duplication) and only if the hidden content is provided elsewhere. Use both attributes to ensure best compatibility across user agents.

<a name="live"></a>
## Announcing changes elsewhere on the page
A screen reader can only focus on one element at a time and is unaware of changes happening elsewhere on the page. This is problematic in modern web applications where performing an action on one part of the page may cause changes or notifications to appear somewhere else.

[WAI-ARIA Live Regions](https://www.w3.org/TR/wai-aria/states_and_properties#attrs_liveregions) allow these changes to be exposed to screen readers so that they can be announced to the user.

Attribute | Effect
:----|:----
`aria-live="polite"` | Sets an element as a live region and will announce changes at the next opportunity.
`aria-live="assertive"` | Sets an element as a live region and will interrupt the current task to notify the user of changes.
`aria-atomic="true"` | Announces the whole live region whenever it changes.
`aria-atomic="false"` | Announces only the part that has changed.

Note that screen reader behaviour can be erratic, particularly with complex interfaces. You may need to experiment with different configurations of markup and `aria-live` attributes in order to achieve the best experience.
