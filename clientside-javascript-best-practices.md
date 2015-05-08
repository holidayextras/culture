
# Clientside Javascript (Backbone) Best Practices

## Purpose of this document

 - Make everyone aware of and discourage clientside **anti**patterns.
 - Make everyone aware of clientside patterns.
 - Provide a point of reference for what good code looks like clientside.
 - Get people writing code in a similar way
 
As with any of the doucments in this repository, this is completely up for discussion and open to new pull requests. If you'd like to see something changed just put in a pull request with your point of view and we can discuss the issue
 
## (Backbone) Favour getters over proxy methods:
When working with Backbone.js, unless we need to modify an attribute, we should use the built-in getters to get the attribute rather than writing an unecessary accessor method.

**Bad:**

```javascript
// Inside of the model
name: function() {
  return this.get('name');
}


// Outside of the model
model.name(); // "Hilton"

```

**Good:**

```javascript
// Inside of the model (nothing...) 

// Outside of the model
model.get('name'); // "Hilton"
```

Using the built in getters means:

 - No extra code in the model.
 - Less cognitive load because we don't need to check what the method is doing.
 - We can tell immediately the attribute hasn't been transformed, because we know `get` just returns the raw attribute.
 
## (Backbone) Don't call render from initialize:
This one is quite simple, don't call `this.render()` from your Backbone view's `initialize` method. It might seem nice to not have to manually call `render`, but it ties the view's rendering to its instantiation, and means that we can't add additional initialize actions in subclasses without it calling `render` half way through its initialization.

**Bad:**

```javascript
initialize: function(options) {
  // Some setup code, subclasses calling this will have a render mid-way through thier initialization :(
  this.render();
}

render: function() {
  // Some rendering code...
}
```

**Good:**

```javascript
initialize: function(options) {
  // Some setup code, subclasses can call this without it rendering :-D
}

render: function() {
  // Some rendering code
}
```
 
 
## Do transform data for presentation using presenters:
If you need to transform a given piece or lots of pieces of data for presentation purposes (i.e. to be used in a template), do this in a presenter.

**Don't put presentation transformations in models:** It ends up bloating the models and gets business logic mixed in with presentation logic. Our models need to be **small** and **portable** between different projects. While tripapp needs to work on pretty much everything, another app may only require presentation logic for a small device. A phone, for example, maybe even a watch...

**Don't put presentation transformations in views:** This sounds weird, but clientside javascript views tend to contain a lot of code that handles interactions with the DOM elements they're responsible for. They may also instantiate other sub-views. Therefore, while they contain a lot of code to do with the DOM, it's usually code handling user interaction, rather than the nitty-gritty of transforming data to HTML/content itself.

Good view rendering should look something like:

```javascript
  render: function() {
    this.$el.html(SomeTemplate({
      booking: this.somePresentedBooking,
      user: this.somePresentedUser
    }));
  }
```

The view is still controlling the rendering to the page, but the transformation of data for presentation is happening somewhere else, leaving the view free to handle user interaction.

## Do use dependency injection:

As an example, we've got a small dependency injection pattern for views going in Tripapp that allows us to:

 - Write less code.
 - Enforce our dependencies (throw an exception if they're not met).
 - Keep track of our instances.

**Bad:** 

```javascript
// In parent view
var childView = new ChildView({
  foo: this.foo,
  bar: this.bar,
  baz: this.baz,
  qux: this.qux,
  quux: this.quux  
});


// In child view
initialize: function(options) {
  this.foo = options.foo,
  this.bar = options.bar,
  this.baz = options.baz,
  this.qux = options.qux
  this.quux = options.quux
}
```

**Good:**

```javascript
// In parent view
var childView = new ChildView(this.injector);

// In child view
dependencies: ['foo','bar','baz','qux','quux']
```

In the bad example, we have to pass in all our dependencies manually, and then manually reassign in the `initialize` method of the child view. If any of those dependencies are undefined, the application will not error and will attempt to carry on its execution, yielding unexpected behaviour.

In the good example, however, we only need to pass down the injector that already contains all of the instances we want to reference around the application. Then, by declaring them as dependencies of the child view, they'll be automatically assigned to `this` and if they're missing an exception will be thrown.

## Do use controllers to set up instances:

Traditionally we've made Backbone views responsible for setup logic (e.g. orchestrating the fetching of products, upgrades and setting up of models/collections). However, this means that we're filling our views with complex set-up logic that isn't strictly to do with anything the view is concerned about.

More recently, each route in the our Backbone apps map to a controller action. Where possible we should set up the our instances in these rather than doing it in the view. An easy way to think about this is to **not pass ids** to views, instead pass the instance itself. For example:


**Bad:**

```javascript
// In our controller
var view = new UpgradeView(this.injector.and({
  upgradeId: this.upgradeId
}));
```

**Good:**

```javascript
// In our controller
this.upgrade = new Upgrade({
  id: upgradeId
});

this.upgrade.fetch().then(this._renderUpgradeView);
```

In the bad example we're leaving the responsibility of fetching the upgrade itself to the view that needs the upgrade. This means setup logic gets stuck in views, and we're likely to have several different views that will need to fetch upgrades. 

In the good example, by keeping all of this setup in the controllers we can pass our view a fully instantiated and fetched upgrade. This means we can keep the views responsible for orchestrating rendering and handling DOM interactions only, and allows us to more easily share upgrade-setup logic between different points in the app without needing to resort to complex mixins.


## Do choose between using composition and inheritance carefully:

There's no hard rule about this, but we should consider carefully [S.O.L.I.D](http://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29) design principles when choosing between using mixins and inheritance. On tripapp at least, we're guilty of violating the 'Single responsibility principle' and 'Liskov Substitution Principle' especially badly in the past leaving the behaviour of some of our subclasses unpredictable or incompatible with their superclass methods.

In these instances, composing behaviour might offer a cleaner and more sensible solution than simply inheriting from something that does *most* of what we want.

## Do throw Errors
In clientside JS, if things go wrong they'll go wrong for one user at a time. Because of this, we like to throw exceptions early when something happens that we don't expect. Our Bugsnag account will pick this up and give us a detailed overview of how often the error is occurring and a stack trace at the point it occurred. 

**Bad:**

```javascript
myFunction: function(expectedParam1, expectedParam2) {
  if (!expectedParam1 || !expectedParam2) return null;
  return (expectedParam1.someThing(expectedParam2) + 10);
}
```

**Good:**

```javascript
myFunction: function(expectedParam1, expectedParam2) {
  if (!expectedParam1 || !expectedParam2) {
   throw new Error('myFunction requires expectedParam1 and an expectedParam2');
  }
  return (expectedParam1.someThing(expectedParam2) + 10);
}
```

In the bad example our application will try to carry on executing and possibly break further on at some point down the line. At that point we may struggle to trace the issue back to here.

In the good example, an error thrown as soon as we realise something's not right will alert us immediately of the issue and make it much easier for us to figure out what's wrong. It may mean one of our users can't use the app, but that's preferable to lots of users getting lots of hard to debug issues because we're afraid of being strict on required parameters.

## Do notify bugsnag with warnings and info messages
Sometimes we'll get to points in our code where we expect certain data to be available in the vast majority of cases, but we know there might be cases when it's not. In these scenarios it's often useful to notify bugsnag with a warning or info message. Doing this allows us to keep tabs on the number of times these edge cases are occurring and lets us easily see when something looks wrong further down the line.

A good example might be a 90 minute results timeout on tripapp. We expect this to happen some of the time, but we know it can lead to unexpected behaviour on the part of the user. By triggering a warning we can get a historical view of the number of results timeouts occurring, along with any additional information right in the bugsnag dashboard.