# Clientside Javascript (jQuery) Best Practices

## Checking The DOM Has Loaded

In most Holiday Extras projects we favour loading jQuery asynchronously so there should be no need to check for the DOM. However in some cases you may still need to, for example creating a custom plugin. The best way to check the DOM has loaded in this instance is to use an immediately invoked function. This gives you a locally scoped `$` to play with.

##### Bad

```
$(document).ready(function(){

  // DOM is ready

});
```

##### Good

```
(function(init) {

  init(window.jQuery, window, document);

}(function($, window, document) {

  $(function() {

    // DOM is ready

  });

}));
```

## jQuery Objects

### Cache jQuery Objects When Repeating

If you plan to repeatedly use a jQuery object, for example if it's called within an event or for loop, then cache it in a variable.

Prefix your variable with a `$` to denote it as a jquery object.

##### Bad

```
$('a').on('click', function(){
  $('div').append('a');
});

```

##### Good

```
var $div = $('div');

$('a').on('click', function(){
  $div.append('a');
});
```

### Chain When Possible

Most jQuery methods will return the current object allowing you to chain them, which you should. In single-use cases this will negate the need to cache the object. Separating each method onto an individual line and indenting will make it easier to read.

##### Bad

```
var $div = $('div');

$div.addClass('a-class');
$div.height('22px');
$div.css('color','#');
$div.removeClass('another-class');
$div.show();
```

##### Good

```
$('div')
  .addClass('a-class')
  .height('22px')
  .css('color','#')
  .removeClass('another-class')
  .show();
```

When traversing the DOM however it is best to break the chain.

##### Bad

```
$('div')
  .children('a-class')
  .hide()
  .parent()
  .children('b-class')
  .css('color','#');
```

##### Good

```
var $div = $('div');

$div
  .children('a-class')
  .hide();

$div
  .children('b-class')
  .css('color','#');
```

## Selectors

### Use Simple Selectors

Pick simple selectors as these will be easier to read and relate to native JavaScript functions.

##### Bad

```
$('.wrapper span.my-element').text();
```

##### Good

```
$('.my-element').text();
```

### IDs Are King

When it comes to simple selectors, using an ID e.g. `$('#my-id')` is the simplest and best way to select a unique element as it will defer to `document.getElementByID`.

Using a tag e.g. `$('div')` will defer to `getElementsByTagName` but this is ambiguous and should only be used when selecting children or descendants of another object.

Using a class e.g. `$('.my-class')` is the least favoured option of the three. Though it will defer to `getElementsByClassName` this is not supported in all older browsers.

### Navigating The DOM

When traversing the DOM, where possible it is best to start with an ID and work your way up/down using `.children()`, `.find()`, `.parent()` etc.

This will improve performance when navigating through dependant, especially in older browsers that don't natively support getElementsByClassName.

It will also help to ensure the use of simple selectors and works best when the parent object is stored in a variable.

##### Bad

```
$('#wrapper > .child').css({ color: '#fff' });
$('#wrapper .descendant').css({ color: '#f00' });
```

##### Good

```
var $wrapper = $('#wrapper');

$wrapper
  .children('.child')
  .css({ color: '#fff' });

$wrapper
  .find('.descendant')
  .css({ color: '#f00' });
```

## Events

### Use `.on()` And `.off()`

The older jQuery methods `bind`, `live` and `delegate` along with the individual event methods, such as `click`, `submit` etc, should not be used to bind events.

Instead use the `on` and `off` methods.

```
$('#my-link').on('click',function(){
  // my special function
});

$('#my-link').off('click');
```

### Multiple Events

When attaching multiple events it is better to pass in an object.

##### Bad

```
$('div').on('mouseenter',function(){
  // mouseenter action
});

$('div').on('mouseleave',function(){
  // mouseleave action
});
```

##### Good

```
$('div').on({
  'mouseenter': function(){
    // mouseenter action
  },
  'mouseleave': function(){
    // mouseleave action
  }
});
```

### Delegate Where Possible

When attaching a single event handler to many elements it is more efficient to attach it to a parent, preferably using an id, and delegate to the child/descendant. When the event propogates up to the parent the handler will then be called on the `eventTarget`.

##### Bad

```
$('#my-list li').on('click',function(){
  // my event
});
```

##### Good

```
$('#my-list').on('click','li',function(){
  // my event
});
```

### Namespace For More Complex Functions

In more complex applications where you may be attaching/detaching many events you should namespace the event but using a full stop after the event name. E.g. `mouseover.my-event`

##### Bad

```
$('a').on('click',function(){
  // first event handler
});

$('a').on('click',function(){
  // second event handler
});

// This will remove all click events attached to the link
$('a').off('click');
```

##### Good

```
$('a').on('click.first',function(){
  // first event handler
});

$('a').on('click.second',function(){
  // second event handler
});

// This will remove only the first handler
$('a').off('click.first');
```
