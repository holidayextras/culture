# Best Practices for Accessible Web Content

**Table of contents**

* [Text alternatives for images](#text-alternatives)
* [Labels on form fields](#labels)


<a name="text-alternatives"></a>
##Text alternatives for images

Every ```img``` element must have an ```alt``` attribute, regardless of whether or not it holds a value.

### Decorative images
If an ``img`` is purely decorative or does not contain any more information than is already available in text, the ```alt``` attribute should be empty. Images with empty ```alt``` attributes are ignored by assistive technology.

In the following example, an image of a telephone is displayed next to a phone number. The heading already provides context and meaning for the number so there is no value in the image being announced as well. The image should therefore have an empty ```alt``` attribute so that it is ignored completely.

```html
<h2>Call our free UK Helpline</h2>
<img src="/images/telephone.png" alt="">
0800 093 5478
```

<a name="labels"></a>
##Labels on form fields

Every form field (apart from buttons) must have a ```label``` that describes its purpose.

Each ```label``` should be explicitly paired with its field by a ```for``` attribute on the ```label``` equal to the ```id``` on the field. Not only does this allow screen readers and other assistive technology to announce the correct label when the field receives focus, it also increases the size of the clickable area for other users.

In the example below, the "Flying from?" label has as ```for="location"``` attribute which explicitly pairs it with the ```select``` control that has a corresponding ```id="location"``` attribute.

```html
<label for="location">Flying from?</label>
<select name="location" id="location">
    <option value="ABZ">Aberdeen</option>
    <option value="BFS">Belfast International</option>
    ...
</select>
```

Placeholder text (a default vaule in the field) should **not** be used instead of a separate ```label``` element. Placeholders are cleared on focus and often not announced at all by screen readers or other assistive technology.

In some cases, it might be appropriate to visually hide a label. For example, where separate fields are provided for the day, month and year of a date, it is often unnecessary to display the label for each. In this situation, it is acceptable to hide the labels with CSS as long as they remain accessible to assistive technology in the HTML.

```html
<fieldset>
    <legend>Date of birth</legend>
    <label for="day">Day</label>
    <input id="day" type="text">
    <label for="month">Month</label>
    <select id="month">
        <option value="jan">January</option>
        <option value="feb">February</option>
        ...
    </select>
    <label for="year">Year</label>
    <input id="year" type="text">
</fieldset>
```

###Checkboxes and radio buttons
As well as being explicitly associated with ```for``` and ```id``` attributes, labels for checkboxes and radio buttons should appear after the control in the markup and be displayed to the right of the control on screen. This convention ensures compatibility with the widest range of assistive technology, and is also consisitent with the UI guidelines established by Microsoft and Apple.

<a name="headings"></a>
##Headings
HTML heading elements (```h1``` to ```h6```) must be used to denote headings so that the structure of the page is clear.

It not sufficient to simply make something look like a heading by, for example, increasing its font size or changing its colour; such changes are purely visual and do not convey any semantic meaning that can be interpreted by assistive technology. Equally, heading elements should not be used to apply styling to content that is not a heading.

Headings should follow a hierarchical structure and levels should not be skipped. For example, an ```h1``` should not be followed by an ```h3``` without an intervening ```h2```.