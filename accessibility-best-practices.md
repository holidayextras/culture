# Best Practices for Accessible Web Content

**Table of contents**

* [Text alternatives for images](#text-alternatives)
* [Labels on form fields](#labels)


<a name="text-alternatives"></a>
##Text alternatives for images

Every ```img``` element must have an ```alt``` attribute;

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