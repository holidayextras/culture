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


## Unit Tests

### Each test should:
* Use `sinon.stub` to limit behaviour
* Assert all return/callback values
* Assert every stub is invoked with all expected parameters
* Use fake variables wherever possible
* Reference stubbed data to ease duplication
* Restore all stubs after the test

Code:
```
someModule._add = function(first, second) { };
someModule._proxy = function(first, second) {
  return someModule._add(first, second);
};
```
Test:
```
var first = 'first', second = 'second', expectedResult = 'result';  // using fake variables by reference
sinon.stub(someModule, '_add').returns(expectedResult);             // stubbing with sinon
var result = someModule._proxy(first, second);

assert.equal(result, expectedResult);                               // assert the return value
sinon.assert.calledWith(someModule._add, first, second);            // assert the stub was invoked as expected
someModule._add.restore();                                          // restore the stub
```

### How many tests should we write?

* One for every path through a function
* One for every error condition through a function
* One with params substitued for `null` (to check it won't crash)

This piece of code should have 4x tests:
```
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

This piece of code should have 2x tests: (clever choice of input value can cover all the code paths)
```
someModule._action = function(number) { // test with `null`

  for (var i=0; i<number; i++) {   // test we loop over here
    if (i%3 == 0) number++;        // test we get here
    number++;
  }

  return number;                   // should always finish here
};
```

#### My function is too complicated, this is too much effort!

Split it up into other functions to reduce the complexity:
```
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
```
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
