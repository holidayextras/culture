# HX Testing Standards

# 1.0 Unit Tests
The aim is for our unit test output to describe the functionality of our code. As long as our tests are readable and clearly structured, we should focus our time on ensuring we have high quality tests. Being able to use the test output as a description for a piece of work, where appropriate, ensures our code is unambiguous and that are tests cover expected functionality. We _should_ be able to use our test output as documentation to our code.

## 1.1 Our Technology
In order to keep test writing familiar across the Holiday Extras group, we should use the following tech for writing our unit tests:

* [Mocha](https://mochajs.org/): Our test framework of choice
* [Sinon](http://sinonjs.org/): For our stubs & spies
* [Assert](https://www.npmjs.com/package/assert) or [Chai](http://chaijs.com/): For making assertions in our tests
* [Sinon-chai](https://github.com/domenic/sinon-chai): For integrating Sinon with Chai
* [chai-as-promised](https://github.com/domenic/chai-as-promised/): For clean promise assertions
* [React Test Utilities](https://facebook.github.io/react/docs/test-utils.html): Helpers for testing React components
* [Enzyme](http://airbnb.io/enzyme/): Test React easily with a jQuery like API.

### Assertion Syntax
With Chai, we always use the `expect` syntax for our tests. Chai + expect provides a clean, readable style of test writing, for example:

```javascript
expect(foo).to.be.a('string');
expect(foo).to.equal('bar');
expect(foo).to.have.length(3);
expect(beverages).to.have.property('tea').with.length(3);
```

And with the addition of sinon-chai for sinon stubs:

```javascript
expect(myStub).to.have.been.calledWith('foo');
```


### Linting Issues
You may occasionally encounter a slight issue here with our linting, specifically our linting complaining about unused expressions in the tests due to Chai's syntax. The solution to this is to use [dirty-chai](https://www.npmjs.com/package/dirty-chai) to provide functions that replace the property getters provided by Chai. This allows us to use syntax such as:

```javascript

expect(myStub).to.have.been.called();
```


## 1.2 Good Test Folders
When adding new tests to projects, test files should sit in a folder hierarchy that mimics the hierarchy fo the source code. The test files themselves should be named similarly to the source files, but with `test` at the beginning of the file name. For example, if we have a source file `src/views/checkoutView.js` we should have a test file `test/unit/views/checkoutView.js` or similar. This makes finding tests an easy and logical process.

The location of the tests directory may differ between existing projects, but for new projects tests should be located in a `test` directory at the root of the project. On existing projects we should work towards having this `test` directory in the root of the project.

## 1.3 Good Test Files
Test files should be clean and readable. This means making sensible use of some of Mocha's features to structure test files

### Using `describe` & `context` blocks
Mocha's `describe` and `context` syntax can be used to split out tests into logical groups. Typically each function in a module would have its own `describe` block, and various paths through that function would have their own `context` block nested within the parent describe block.


### Using `it` blocks
Within describe blocks we can put `it` blocks, it's within these `it` blocks that we put the assertion for the thing we're testing. Keeping to **one** assertion per `it` block makes pinpointing the source of a test failure very easy, so this is considered good practice when appropriate. For example, having multiple assertions in one `it` block might acceptable when testing computationally expensive tasks or assertions of multiple calls of the same function.

Acceptable

```
it('returns x', function(err, result) {
  expect(err).to.not.be.ok();
  expect(result).to.equal('x');
});
```

Not Recommended

```
it('returns x, calls y and err equals foo', function(err, result) {
  expect(err).to.equal('foo');
  expect(result).to.equal('x');
  expect(y).to.have.been.called();
});
```

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

### Sinon Sandbox

Using **sinon.sandbox** object to hold your stubs/spies. This allows you to be able to put multiple stubs within an object when creating them in a **beforeEach** and instead of restoring them one at a time, you can restore them all at once in the **afterEach**. This means we can restore all of our stubs/spies at once.

```javascript
var sandbox = sinon.sandbox.create();

 beforeEach(function() {
  sandbox.stub(obj1, 'method1');
  sandbox.stub(obj1, 'method2');
  sandbox.stub(obj2, 'method3');
  sandbox.stub(obj2, 'method4');
  sandbox.spy(obj3, 'method5');
 });
```

 ```javascript
 afterEach(function() {
  sandbox.restore();
 });
```

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
* Making use of the [dependency injection pattern](https://en.wikipedia.org/wiki/Dependency_injection) can help to make your code more easily testable. If you have a class that requires a connector for some database, for example, injecting that database dependency when you create an instance of your class means it's simple to swap out for a mock dependency when you come to test the class. The alternative, building the dependency right into the class, would involve ruthless stubbing of a real database connector which puts you at risk of tests becoming brittle, as well as running code beyond the scope of your unit test. This would also mean your class was then tightly coupled to a single database connector type, making it harder to swap out in the future.

## 1.6 Unit Testing Gotchas

### Hard to test functions
Split it up into other functions to reduce the complexity:

```javascript
someModule._action = function(number) {
  for (var i=0; i<number; i++) {
    if (i%3 == 0) {
      number++;
    }
    
    if (i>4) {
      number++;
    }
    number++;
  }
  return number;
};
```

Becomes:

```javascript
someModule._numberLoop = function(number) {
  if (i%3 == 0) {
    number++;
  }
  
  if (i>4) {
    number++;
  }
   
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
  });
```

### Testing side effects of promises

Remember to include a `catch` in your test if you've put expectations of a promise's side effects inside a `then`

```javascript
  // Bad - your expectation might throw an error, which would be automatically caught.
  // The test will never call `done`, and timeout instead of explaining why it failed
  it('opens an alert', function(done) {
    promisedOpenAlert().then(function() {
      expect(window.alert).to.have.been.called();
      done();
    });
  });

  // Better - if `done` is called with an error argument it will fail your test with the
  // details of the error; so you can pass it directly into `catch`
  it('opens an alert', function(done) {
    promisedOpenAlert().then(function() {
      expect(window.alert).to.have.been.called();
      done()
    }).catch(done);
  });
```

## 1.7 Testing React

### Enzyme
Using Enzyme makes testing React components easier:

#### Shallow rendering

**With React TestUtils:**
```javascript
beforeEach(function() {
  var shallowRenderer = TestUtils.createRenderer();
  shallowRenderer.render(<Page {...props} />);
  renderedPage = shallowRenderer.getRenderOutput();
});

context('page header', function() {

  var renderedHeader = null;

  beforeEach(function() {
    renderedHeader = renderedPage.props.children[0];
  });

  context('h2', function() {

    var h2 = null;

    beforeEach(function() {
      h2 = renderedHeader.props.children[0];
    });

    it('contains a translation', function() {
      expect(h2.props.children).to.equal('a translation');
    });

  });

});
```

**With Enzyme:**
```javascript
beforeEach(function() {
  this.wrapper = shallow(<Page {...props} />);
});

context('page header', function() {

  context('h2', function() {

    it('contains a translation', function() {
      expect(this.wrapper.find('h2').text()).to.equal('a translation');
    });

  });

});
```
As you can see our `beforeEach` block is now much simpler. With the help of [Enzyme](https://airbnb.io/enzyme)'s jQuery like API we're able to find our `h2` element via readable selectors rather than repeating `props.children` many times.

This is just one example of using [Enzyme](https://airbnb.io/enzyme) to make React testing more readable and concise. I would encourage everyone using [Enzyme](https://airbnb.io/enzyme) to read the well written [documentation](http://airbnb.io/enzyme/docs/api/index.html).

# 2.0 System Tests

## Each test should:
* Use product codes that are guaranteed to be available
* Use dynamic dates that are a fixed amount of time in the future
* Not assert against any dynamic data (eg. content) that may change unexpectedly. Instead, fixtures should be used to remove the dependency on any backing services for our tests.

## How many tests should we write?
* Only enough to cover expected customer journeys from start to finish
