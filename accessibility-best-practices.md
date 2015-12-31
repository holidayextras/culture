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

Every form field must have a descriptive label.

<a name="headings"></a>
##Headings
HTML heading elements (```h1``` to ```h6```) must be used to denote headings so that the structure of the page is clear.

It not sufficient to simply make something look like a heading by, for example, increasing its font size or changing its colour; such changes are purely visual and do not convey any semantic meaning that can be interpreted by assistive technology.

Headings should follow a hierarchical structure and levels should not be skipped. For example, an ```h1``` should not be followed by an ```h3``` without an intervening ```h2```.