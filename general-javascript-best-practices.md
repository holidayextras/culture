# General Javascript Best Practices

## Purpose of this document

- Make everyone aware of and discourage general antipatterns.
- Make everyone aware of general patterns.
- Provide a point of reference for what good code looks like at Hx.
- Get people writing code in a similar way.

Please also see the respective serverside and clientside best practice docs.

## Function names should reflect behaviour (as much as possible): 
When writing functions, their names should give a good indication of what they do, for example:

**Bad:**

```javascript
// Function name is misleading.
var notifyBookingComplete = function() {
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