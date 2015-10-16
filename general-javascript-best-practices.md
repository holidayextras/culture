# General Javascript Best Practices

## Purpose of this document

- Make everyone aware of and discourage general antipatterns.
- Make everyone aware of general patterns.
- Provide a point of reference for what good code looks like at Hx.
- Get people writing code in a similar way.

Please also see the respective serverside and clientside best practice docs.

## ECMAScript version support

We use up to ECMAScript 6 at HX. This is fulfilled by Babel and Node v4 where applicable. We should use newer es6 features where it makes sense and can improve readability. Please have a look at our technical resources [section on ES6](technical-resources.md#es6) to familiarise yourself with some of the new features available to us. If we come across any new anti-patterns that arise from using ES6 features we can add them to this document.

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
