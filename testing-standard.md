# HX Testing Standards

# 1.0 Unit Tests

## 1.1 Our Technology
In order to keep test writing familiar across the Holiday Extras group, we should use the following tech for writing our unit tests:

* [Mocha](https://mochajs.org/): Our test framework of choice
* [Sinon](http://sinonjs.org/): For our stubs & spies
* [Chai](http://chaijs.com/): For making assertions in our tests
  * [Sinon-chai](https://github.com/domenic/sinon-chai): For integrating Sinon with Chai
  * [chai-as-promised](https://github.com/domenic/chai-as-promised/): For clean promise assertions
* [React Test Utilities](https://facebook.github.io/react/docs/test-utils.html): Helpers for testing React components 

## 1.2 Good Test Folders
When addind new tests to projects, test files should sit in a folder hierarchy that mimics the hierarchy fo the source code. The test files themselves should be named similarly to the source files, but with `test` at the beginning of the file name. For example, if we have a source file `src/views/checkoutView.js` we should have a test file `test/unit/views/testCheckoutView.js` or similar. This makes finding tests an easy and logical process.

The location of the tests directory may differ between existing projects, but for new projects tests should be located in a `test` directory at the root of the project. On existing projects we should work towards having this `test` directory in the root of the project.

## 1.3 Good Test Files
Test files should be clean and readable. This means making sensible use of some of Mocha's features to structure test files

### Using `describe` & `context` blocks
Mocha's `describe` and `context` syntax can be used to split out tests into logical groups. Typically each function in a module would have its own `describe` block, and various paths through that function would have their own `context` block nested within the parent describe block. 


### Using `it` blocks
Within describe blocks we can put `it` blocks, it's within these `it` blocks that we put the assertion for the thing we're testing. Keeping to **one** assertion per `it` block makes pinpointing the source of a test failure very easy, so this is considered good practice.

### The `beforeEach(callback)` function
Within `describe` and `context` blocks we can call the `beforeEach` function with a callback we want to run before each test is run. This is useful for setting up your test configuration so that your `it` blocks only contain very core of your test; the call to the function being tested and the expected result. This is considered good practice as it makes it very easy to see what's broken when a test fails. There's also an `afterEach(callback)` function which runs after each test - this is useful for tearing down stubs and generally restoring things to a pre-test state.

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

### How many tests should I write?
In short you should write a test for:

* Every path through a function
* Every error condition through a function
* ***When writing serverside tests***, you should write one with params substitued for `null` to check that the function doesn't throw an Error. This is not a necessary consideration on clientside Javascript, where we'd rather fail fast than continue in a erroneous state.

This piece of code should have 3 tests, with an extra null test on the server:

```javascript
someModule._action = function(number) { // test with `null` on server
  if (number == 0) {
    return 1;        // test we get here
  }

  if (number == 1) {
    return 3;        // test we get here
  }

  return 5;          // test we get here
};
```

## 1.4 Good Stubs and Spies
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

## 1.5 Good Unit Test Practice
Now that we've covered good folder structure, good test file structure and the purpose of stubs and spies, we've got everything we need to begin writing good tests.

### Within test suites in general:

* Use fixture files with care. They're sensible for large data objects but usually not necessary.
* Scope variables as close to the tests they're used in as possible (usually at the top of the current `context` block.

### Within `beforeEach` and `afterEach` functions:

* Use `sinon.stub` to limit behaviour with the `beforeEach(callback)` function, `it` block.
* Restore all stubs after each test with the `afterEach(callback)` function
* Reference variables for expected results to ease duplication.

### Within `it` blocks:

* Check all return/callback values
* Check every stub is invoked with all expected parameters

### Generally:

* Unit tests should not rely on any external services to run. If you can't run your unit tests without internet access, there's something wrong with your unit tests. Instead you should use stubs and fixtured data to remove the dependency on external services for your tests.

## 1.6 Unit Testing Gotchas

### Hard to test functions
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

Becomes:

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

### Testing the result of callbacks
Mocha makes this easy out of the box, we can pass a `done` callback to each `it` block if we're running asynchronous code in our test. For example:

```javascript
  it('the function calls back with the number 1337', function(done) {
    myTestFunctionCall(function(result) {
      expect(result).toEqual(1337);
      done();
    });
  ); 
```

### Testing the result of promises
Using the [chai-as-promised](https://github.com/domenic/chai-as-promised/) library makes this very easy:

```javascript
  it('the function calls back with the number 1337', function() {
    expect(myTestFunctionCall()).to.eventually.equal(1337);
  ); 
```

# 3.0 System Tests

## Each test should:
* Use product codes that are guaranteed to be available
* Use dynamic dates that are a fixed amount of time in the future
* Not assert against any dynamic data (eg. content) that may change unexpectedly

## How many tests should we write?
* Only enough to cover expected customer journeys from start to finish