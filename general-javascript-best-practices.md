# General Javascript Best Practices

## Purpose of this document

- Make everyone aware of and discourage general antipatterns.
- Make everyone aware of general patterns.
- Provide a point of reference for what good code looks like at Hx.
- Get people writing code in a similar way.

Please also see the respective serverside and clientside best practice docs.

## ECMAScript version support

We use *some* ECMAScript 6 at HX. This is fulfilled by Babel and Node v4 where applicable. We should use these newer es6 features where it makes sense and can improve readability. Please have a look at our technical resources [section on ES6](technical-resources.md#es6) to familiarise yourself with some of the new features available to us. Please see [below](#what-antipatterns-could-i-run-into-with-my-newfound-es6-power) for advice when using some ES6 features. If we come across any new anti-patterns that arise from using ES6 features we should add them to this document.

## Function names should reflect behaviour (as much as possible):
When writing functions, their names should give a good indication of what they do, for example:

**Bad:**

```javascript
// Function name is misleading.
var notifyThingies = function() {
  if (userLoggedIn) {
    trigger("booking_complete");
  }
}

notifyThingies();
```

**Good:**

```javascript
// Check the condition outside the function:
var notifyBookingComplete = function() {
   trigger("booking_complete");
}

if (userLoggedIn) {
  notifyBookingComplete();
}
```

**Good:**

```javascript
// Adjusting the function name to accommodate the conditional behaviour:
var notifyBookingCompleteIfAuthenticated = function() {
  if (userLoggedIn) {
    trigger("booking_complete");
  }
}

notifyBookingCompleteIfAuthenticated();
```

**Good:**

```javascript
// With a more complex condition, use an external function:
var userLoggedIn = function() {
  return (user.get('email') && session.get('authenticated'));
}

var notifyBookingComplete = function() {
   trigger("booking_complete");
}

if (userLoggedIn()) {
  notifyBookingComplete();
}

```

You can see above that in the bad example, the `notifyBookingComplete()` function suggests it's going to notify some thingies, but that notification is actually condition. If the programmer weren't to read the code in that function they've have no idea, in essence, the function name doesn't accurately reflect the function.

We avoid this issue in the good examples. Depending on the complexity of the condition in question it might be worth updating the function name, moving the condition out of the function or putting the condition into it's own function. These aid with understanding, readability and testability of code.

**Caveat:** This approach isn't always practical with more complex functions. For example, a function may contain complex conditional logic that can't be succinctly reflected in its name, and that we don't want to inline and repeat everywhere. In this scenario, we can use things like throwing `Errors` or `true`/`false` return values for sync code and rejecting promises & erroring callbacks for async code. The ultimate aim is just to ensure that the programmer isn't mislead about the behaviour of the function, and encourage them to consider handling non-happy path behaviour.

## Preferred libraries

There are many 3rd party libraries to use on [npm.org](http://www.npm.org), but some are better than others. Any library listed below should be used over inferior competing libraries:

### Lodash

Use instead of underscore, due to correct semantic versioning, better performance and some extra features.

Please only require the methods you need rather than the whole library in order to keep build sizes at a minimum.

## What antipatterns could I run into with my newfound ES6 power?

### Auto Destructuring
https://leanpub.com/understandinges6/read#leanpub-auto-destructuring

General rule of thumb here is to use this feature as a consumer, not a producer. You shouldn't be required to destructure the result of a function to use it.

The case for destructuring function return values (for example, returning a tuple), can be more of a BAD practice than a good one. Consider this function:
```
function getData() { return [ "oliver", 27 ]; }
```
As a consumer of this function I'll get an array of two values. Without looking at the code, or hunting for documentation, I've got no idea what those data items mean. After some digging I might be able to destructure it like so:
```
var name, age;
[name, age] = getData();
```
and from then on, all future consumers of the original function will have to go through the same process of "what does this data mean?!"

The far better practice, that we currently implement, is whereby we always return a named object:
```
function getData() {
  return {
    name: "oliver"
    age: "27"
  };
}
```
any consumer of this module can very quickly tell what data is coming back, and without looking at the code, just looking at the return value, it's extremely obvious what data they've got.


### Auto Rest parameters + Spread operator
https://leanpub.com/understandinges6/read#leanpub-auto-rest-parameters
https://leanpub.com/understandinges6/read#leanpub-auto-the-spread-operator

```
// Great use case
render: function(editing) {
    return <SessionHeader {...this._headerProps()} />;
  },

  _headerProps: function() {
    return {
      locales: [window.locale],
      onLogoutClick: this._onLogoutClick.bind(this)
    };
  }
```

Don't use it in node because we are currently always explicit in naming function arguments and reducing the params to each function:
```
var doStuff = function(name, list) { /* ... */ };

var name = "oli";
var list = [ 1, 4, 5, 7, 9 ];
doStuff(name, list);
```

## Module Template

All modules should follow this order:

1. define a local object and export it
2. `require()` in any dependencies
3. (all the logic goes here)

For example:
```javascript
// define a local object and export it
var ourModuleName = module.exports = { };

// Next, require any dependencies:
var _ = require('underscore');
var moment = require('moment');
var async = require('async');

// Now attach all functionality to our local object:
ourModuleName.somePublicFunction = function() { };
ourModuleName._somePrivateFunction = function() { };
```

In the above example its easy to identify a module's dependencies and the module's exports without having to scroll anywhere. It also sets the scene to attach any public and private functions to the exported object to enable testing and it means all functions can be pushed up against the start of the line with no unnecessary indentation.
