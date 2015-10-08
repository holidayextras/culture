# HX Testing Standards

## Selenuim Tests

TODO

## System Tests

### Each test should:
* Use product codes that are guaranteed to be available
* Use dynamic dates that are a fixed amount of time in the future
* Not assert against any dynamic data (eg. content) that may change unexpectedly

### How many tests should we write?
* Only enough to cover expected customer journeys from start to finish


# Unit Tests

## Our Technology
In order to keep test writing familiar across the Holiday Extras group, we should use the following tech for writing our unit tests:

* [Mocha](https://mochajs.org/): Our test framework of choice
* [Sinon](http://sinonjs.org/): For our stubs & spies
* [Chai](http://chaijs.com/): For making assertions in our tests
* [Sinon-chai](https://github.com/domenic/sinon-chai): For integrating Sinon with Chai
* [React Test Utilities](https://facebook.github.io/react/docs/test-utils.html): Helpers for testing React components 

## Good File & Folder Structures
When addind new tests to projects, test files should sit in a folder hierarchy that mimics the hierarchy fo the source code. The test files themselves should be named similarly to the source files, but with `test` at the beginning of the file name. For example, if we have a source file `src/views/checkoutView.js` we should have a test file `test/unit/views/testCheckoutView.js` or similar. This makes finding tests an easy and logical process.

The location of the tests directory may differ between existing projects, but for new projects tests should be located in a `test` directory at the root of the project. On existing projects we should work towards having this `test` directory in the root of the project.

## Good Test Structure
Test files should be clean and readable. This means making sensible use of some of Mocha's features to structure test files

### Using `describe` & `context` blocks
Mocha's `describe` and `context` syntax can be used to split out tests into logical groups. Typically each function in a module would have its own `describe` block, and various paths through that function would have their own `context` block nested within the parent describe block. 


### Using `it` blocks
Within describe blocks we can put `it` blocks, it's within these `it` blocks that we put the assertion for the thing we're testing. Keeping to **one** assertion per `it` block makes pinpointing the source of a test failure very easy, so this is considered good practice.

### The `beforeEach(callback)` function
Within `describe` and `context` blocks we can call the `beforeEach` function with a callback we want to run before each test is run. This is useful for setting up your test configuration so that your `it` blocks only contain very core of your test; the call to the function being tested and the expected result. This is considered good practice as it makes it very easy to see what's broken when a test fails.

Piecing the above together, for the following fake module:

```
module.exports = {
  prettyFullName: function(firstName, lastName) {
    if (!firstName) {
      return '';
    }
    
    if (!lastName) {
      return firstName;
    }
    
    return firstName + ' ' + lastName;
  }
}
```

We'd end up writing the following test suite:

```javascript
var nameHelper = require('helpers/nameHelper.js');

describe('nameHelper', function() {
  
  describe('prettyFullName(firstName, lastName)', function() {
  
  	  var testFirstName;
  	  var testLastName;
  
  	  beforeEach(function() {
  	    testFirstName = 'Geoffrey';
  	    testLastName = 'Banks';
  	  });

	  context('when no first name is provided', function() {

       it('returns an empty string', function() {
         // Test that nameHelper.prettyFullName() returns an empty string
       });
       	    
     });
	
	  context('when no last name is provided', function() {
	  
       it('returns the first name', function() {
         // Test that nameHelper.prettyFullName(testFirstName) returns 'Geoffrey'
       });

     });
	
     context('When both the first name and last name are provided', function() {

       it('returns the first name and the last name concatenated', function() {
         // Test that nameHelper.prettyFullName(testFirstName, testLastName) returns 'Geoffrey Banks'
       });

	  });

  });

});
```

This looks fairly verbose, but it is a contrived example. In the real world when testing modules with many functions of varying degrees of complexity, this structure _saves_ code and keeps people having to read the tests sane.

## Good Stubs and Spies
Stubs and Spies are similar but subtly different. In our unit tests we should stub wherever possible, but to know why we first need to understand the difference.

Take a very basic function:

```javascript
var onePlusOne = function() {
  return 1+1;
};
```

### Spies
If we ***spied*** on this function, we'd be able to tell when `onePlusOne` had been called, and how many times, and what with, but we'd always end up executing the code inside the function and it'd always return `2`.

### Stubs
If we ***stubbed*** this function, we'd be able to do everything a spy does, but the body of the function code would never actually be called. Instead it gets replaced by the stub, and immediately returns any value we want. So for our test, we could make our `onePlusOne` function return `5`, for example.

Stubs are really the bread and butter of unit testing, because they allow us to fake interactions within our codebase and focus only on testing the code we care about the code defined in the function. This keeps test setup short, keeps tests understandble and makes their results predictable.

Confused? Take the following function:

```javascript
var hoursAlive: function() {
  var birthHours = getHoursAtBirth();
  var deathHours = getHoursAtDeath();
  return deathHours - birthHours;
};
```

There are three things we should check for in the above function:

 1. That we retrieve the birthHours.
 2. That we retrieve the deathHours.
 3. That we correctly calculate the hours alive based on the two.

By stubbing `getHoursAtBirth` and `getHoursAtDeath` we can **a)** check that both of these are called (ticking off points 1 and 2 above), and **b)** ensure that the real versions of `getHoursAtBirth` and `getHoursAtDeath` are never actually called. This is crucially important as we're **not testing that those functions work**. We only care that they've been called and that what we do in the `hoursAlive` function works. 

Other tests will check that `getHoursAtBirth` and `getHoursAtDeath` work, and by building up a suite of tests that ensure each function works as it's supposed to, we can build confidence that our codebase works correctly. It's for this reason that `stubs` are preferred to spies in our unit tests - we don't want to be running code outside of the function we're actually testing during a unit test.

## Each test should:
* Use `sinon.stub` to limit behaviour
* Assert all return/callback values
* Assert every stub is invoked with all expected parameters
* Use fake variables wherever possible
* Reference stubbed data to ease duplication
* Restore all stubs after the test

Code:

```javascript
someModule._add = function(first, second) { };
someModule._proxy = function(first, second) {
  return someModule._add(first, second);
};
```

Test:

```javascript
var first = 'first', second = 'second', expectedResult = 'result';  // using fake variables by reference
sinon.stub(someModule, '_add').returns(expectedResult);             // stubbing with sinon
var result = someModule._proxy(first, second);

assert.equal(result, expectedResult);                               // assert the return value
sinon.assert.calledWith(someModule._add, first, second);            // assert the stub was invoked as expected
someModule._add.restore();                                          // restore the stub
```

## How many tests should we write?

* One for every path through a function
* One for every error condition through a function
* One with params substitued for `null` (to check it won't crash)

This piece of code should have 4x tests:

```javascript
someModule._action = function(number) { // test with `null`
  if (number == 0) {
    return 1;        // test we get here
  }

  if (number == 1) {
    return 3;        // test we get here
  }

  return 5;          // test we get here
};
```

This piece of code should have 2x tests: (clever choice of input value can cover all the code paths):

```javascript
someModule._action = function(number) { // test with `null`

  for (var i=0; i<number; i++) {   // test we loop over here
    if (i%3 == 0) number++;        // test we get here
    number++;
  }

  return number;                   // should always finish here
};
```

### My function is too complicated, this is too much effort!

Split it up into other functions to reduce the complexity:

```javascript
someModule._action = function(number) {
  for (var i=0; i<number; i++) {
    if (i%3 == 0) number++;
    if (i>4) number++;
    number++;
  }
  return number;
};
```

becomes:

```javascript
someModule._numberLoop = function(number) {
  if (i%3 == 0) number++;
  if (i>4) number++;
  number++;
  return number;
};

someModule._action = function(number) {
  for (var i=0; i<number; i++) {
    number = someModule._numberLoop(number);
  }
  return number;
};
```
