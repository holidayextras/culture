
# Server Side Javascript Best Practices

## Purpose of this document

* To provide guidance to developers who are new to writing modules
* To help us write more testable code
* To make it easier for others to work with our code
* To maintain consistent quality across all our projects
* To provide a referencable document when performing code review
* To give us more confidence when making code changes






## Tests

We aim to have:

* 90% unit test coverage.
* External system tests covering the most important code paths
* Supplier tests to allow Zabbix alerting on critical infrastructure
* Health check tests to identify when a service is healthy enough to serve production traffic






## Documentation
Every new module should have an accompanying `documentation.md` which should cover:

* The purpose of the module
* The public functions exposed by the module
  * Function name
  * Function parameters
  * return / callback values
  * High level overview of any business processes

With this context a module is considered to either be a fully blown npm-module, or any folder containing an `index.js`.






## Modules
Any piece of logically distinct code that we share between other projects should be a candidate to move out into a real (private) npm module. In order to reduce the complexity of our codebases we prefer to avoid submoduling too soon - only when code is needed elsewhere should it be considered for moduling. A module should contain, at bare minimum:

* README.md
* documentation.md
* package.json
  * name
  * git url
  * version number
  * dependencies






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





## Promises or Callbacks?

Provided the orchestration of asynchronous actions are decoupled from the code that provides functionality, both conventions are acceptable. For example, both of the following examples are great:

```javascript
ourModuleName._orchestrate = function() {
  return ourModuleName._firstThing()
    .then(ourModuleName._secondThing)
    .then(ourModuleName._thirdThing)
    .then(ourModuleName._fourthThing)
    .fail(function(err) {
      //
    });
};
```
```javascript
ourModuleName._orchestrate = function(callback) {
  async.waterfall([
    ourModuleName._firstThing,
    ourModuleName._secondThing,
    ourModuleName._thirdThing,
    ourModuleName._fourthThing
  ], callback);
};
```





## Return Early

* Each logic block is isolated
* There is less state being carried around
* Its easy write concise tests
* (Ideally) One unit test per return statement
* Its easier to identify missing test coverage

Bad
```javascript
var result = null;
if (a == 5) {
  result = 1;
} else if (b == 7) {
  result = 2;
} else {
  result = 3;
}
return result;
```
Good
```javascript
if (a == 5) {
  return 1;
}

if (b == 7) {
  return 2;
}

return 3;
```






## Keep the logic simple

* Be kind to our fellow developers, keep it simple
* Simple logic is easier to understand and more likely to be correct
* Nobody wants to spend 10 minutes trying to read one line of code

Bad
```javascript
if (a && !b && (!(c || d) || e)) {
  // stuff goes here
}

if (( a && b ) || ( a && c )) {
  // the copy + paste jobbie
}
```
Good
```javascript
if (!a || b) return;
if (!c && !d) return;
if (!e) return;

// stuff goes here
```






## Internal private functions should be exposed to enable testing

* Private functions need to be exported to allow us to stub them
* Private functions should be prefixed by `_`s
* Every private function should be exported
* A priority list of of desirable tests, from most to least important, is as follows:
** End-to-end tests proving core functionality
** Proving the module won't crash given bad inputs
** Common edge cases within the module
** Any non-obvious logic

Bad
```javascript
var myModule = { };
module.exports = myModule;

myModule.mainFunctionality = function() {
  // chain other private functions together
};

function private1() { };
function private1Part2() { };
function private2() { };
function private3() { };
```
Good
```javascript
var myModule = { };
module.exports = myModule;

myModule.mainFunctionality = function() {
  // chain other private functions together
};

myModule._private1 = function() { };
myModule.__private1Part2 = function() { };
myModule._private2 = function() { };
myModule._private3 = function() { };
```






## Functions should have a single responsibility

* Results in easily testable code
* Its easier to confirm the behaviour of a single responsibility function
* Short functions require less stubbing, making tests easier to write

Bad
```javascript
var functionality = function(p1, p2, callback) {
  cache.get(p1, p2, function(err, result) {
    if (!err && result) return callback(result);

    var a = p1 + p2;
    var b = a > 5 ? p1 + 1 : p2 - 4;

    if (b > 10) {
      extService.b(p1, p2, function(err, result) {
        return callback(null, result + 2);
      });
    } else {
      extService.a(p1, p2, function(err, result) {
        return callback(null, result + 1);
      });
    };
  });
};
```
Good
```javascript
obj._checkCache = function(p1, p2, callback) {
  return cache.get(p1, p2, callback);
};

obj._preprocess = function(p1, p2, callback) {
  var a = p1 + p2;
  return callback(null, a);
};

obj._request = function(p1, p2, callback) {
  if (p1 < 10) {
    return extService.carparkSearch(p1, p2, callback);
  }
  return extService.refund(p1, p2, callback);
};
```






## Handling heavily sequential async code with callbacks

* `async.waterfall` allows us to chain functions together
* !! See the appendix at the end for a more testable version of this example.

```javascript
createBooking._orchestrate = function(data, callback) {
  async.waterfall([
    function(callback) { return callback(null, data); },
    createBooking._sendBook1Request,
    createBooking._generatePaymentRequest,
    createBooking._processPayment,
    createBooking._sendBook2Request,
    createBooking._generateBookingReference
  ], callback);
};
```
```javascript
var sandbox = sinon.sandbox.create();
sandbox.stub(createBooking, '_sendBook1Request').yields(null, 'data1');
sandbox.stub(createBooking, '_generatePaymentRequest').yields(null, 'data2');
sandbox.stub(createBooking, '_processPayment').yields(null, 'data3');
sandbox.stub(createBooking, '_sendBook2Request').yields(null, 'data4');
sandbox.stub(createBooking, '_generateBookingReference').yields(null, 'data5');

createBooking._orchestrate('data0', function(err, result) {
  assert.equal(err, null);
  assert.equal(result, 'data5');

  assert.ok(createBooking._sendBook1Request.calledWith('data0'));
  assert.ok(createBooking._generatePaymentRequest.calledWith('data1'));
  assert.ok(createBooking._processPayment.calledWith('data2'));
  assert.ok(createBooking._sendBook2Request.calledWith('data3'));
  assert.ok(createBooking._generateBookingReference.calledWith('data4'));

  sandbox.restore();
  console.log("Pass");
});
```





## Handling heavily sequential async code with promises

* !! See the appendix at the end for caveats with simple promise chaining.
* The following test example uses `sinon-bluebird` to enable stubbing of promises. Any stubbing implementation is acceptable provided it allows us to control the data the stub resolves/fails with and provided we can assert against the data a stub is invoked with.

```javascript
createBooking._orchestrate = function(data) {
  return createBooking._sendBook1Request(data)
    .then(createBooking._generatePaymentRequest)
    .then(createBooking._processPayment)
    .then(createBooking._sendBook2Request)
    .then(createBooking._generateBookingReference);
};
```
```javascript
require('sinon-bluebird');

sinon.stub(createBooking, '_sendBook1Request').resolves('data1');
sinon.stub(createBooking, '_generatePaymentRequest').resolves('data2');
sinon.stub(createBooking, '_processPayment').resolves('data3');
sinon.stub(createBooking, '_sendBook2Request').resolves('data4');
sinon.stub(createBooking, '_generateBookingReference').resolves('data5');

var data = 'data0';

createBooking._orchestrate(data).then(function(result) {
  assert.equal(result, 'data5');

  assert.ok(createBooking._sendBook1Request.calledWithPromise('data0'));
  assert.ok(createBooking._generatePaymentRequest.calledWithPromise('data1'));
  assert.ok(createBooking._processPayment.calledWithPromise('data2'));
  assert.ok(createBooking._sendBook2Request.calledWithPromise('data3'));
  assert.ok(createBooking._generateBookingReference.calledWithPromise('data4'));

  sinon.restore(createBooking._sendBook1Request);
  sinon.restore(createBooking._generatePaymentRequest);
  sinon.restore(createBooking._processPayment);
  sinon.restore(createBooking._sendBook2Request);
  sinon.restore(createBooking._generateBookingReference);
  done();
});
```





## Functions must either return data OR callback with data

* Synchronous functions must return either a truthy value, or `null`
* Async functions must callback with `(err, someDataObject)`
* Never accept a return value from an async function

```javascript
var mySequentialFunction = function(a, b, c) {

  return d;
};
```
```javascript
var myAsyncFunction = function(a, b, callback) {

  return callback(null, d);
};
```





## Data returned from a function must be explicit

* It should be clear what data we're expecting a function to provide back to its callee.
* Here are 2x very contrived examples to demonstrate the difference:

Bad (what data flows back to the callee??):
```javascript
// Callback pattern
return database.query(data, callback);

// Promise pattern
return database.query(data);
```
Good:
```javascript
// Callback pattern
database.query(data, function(err, result) {
  if (err) return callback(err);
  return callback(null, result);
});

// Promise pattern
return database.query(data).then(function(result) {
  return Promise.resolve(result);
});
```





## Error Handling

In the ideal world:

* We validate input parameters enough to ensure we don't crash, but no further
* We handle errors on every anyonymous function
* Errors should bubble upwards
* If no data is going to be provided to a callback, an error should be given

```javascript
var ourIdealFunction = function(params, callback) {
  if (!(callback instanceof Function)) throw new Error('Missing callback!');
  if (!params) return callback(‘Invalid params’);

  agentLookup({ code: params.agent }, function(err, result) {
    if (err || !result) return callback(‘Unable to lookup agent’);

    var output = transformAgentLookup(result);
    return callback(null, output);
  });
};
```






## Passing params between modules

* When interacting with another module, never proxy objects onwards
* Proxied objects are a black-box mystery
* Always redefine the properties we're expecting to pass on
* It makes it easier for other developers to work on others modules

Bad
```javascript
controller.doStuff = function(params, callback) {
  // do stuff ..
  supplier.send(params, function(err, result) {
    return callback( );
  });
});
```
Good
```javascript
controller.doStuff = function(params, callback) {
  var supplierParams = {
    param1: params.param1,
    param2: params.param2 || null,
    param3: params.flight ?
              params.flight.transform( ) :
              null
  };
  supplier.send(supplierParams, function(err, result) {
    return callback( );
  });
});
```






## Connection Handling

When dealing with external connections, ensure resources are released when they're no longer needed.

Bad
```javascript
var connectionFactory = require('something');

var doStuff = function(callback) {
  connectionFactory.create(function(connection) {
    connection.find('something', function(err, data) {

      // When this returns, the connection is left hanging forever
      return callback(err, data);
    });
  });
};
```
Good
```javascript
var connectionFactory = require('something');

var doStuff = function(callback) {
  connectionFactory.create(function(connection) {
    connection.find('something', function(err, data) {

      connection.release();  // <-- free the connection!
      return callback(err, data);
    });
  });
};
```






## We don't use native Error()'s

Constructing an Error() cookie-cutters properties:
```javascript
> var tmp1 = new Error('Message1');
> tmp1.errorCode = 404;
> var tmp2 = new Error(tmp1);
> tmp2.errorCode;
undefined
```
Error()s don't stringify into log files:
```javascript
> var tmp1 = new Error('Message1');
> JSON.stringify(tmp1);
'{ }'
```
Our synchronous functions return truthy values or nulll, never Error()s:
```javascript
var add = function(a, b) {

  return a || null;
};

var result = add(1, null);
if (!result) return callback(‘Couldnt add numbers’);
```
Callbacks get error data as the first param, their type is irrelevant:
```javascript
var something = function(params, callback) {
  if (err) return('Something');

});

something(1, function(err, result) {
  // in here, err is the error...
});
```






## Avoid try + catch

* If code deschedules, any thrown errors will not be caught by a try+catch block

```javascript
try {
  someAsyncFunc(function(err) {
    if (err) throw err; // this will not be caught

  });
} catch (err) {
  console.log(err);
}
```

* The only exception to avoiding try+catch is with JSON.parse:

```javascript
try {
  json = JSON.parse(json);
} catch(e) { }
```






## Be extremely careful when using `throw`

* Our main use case for throw'ing errors is via public exported functions to ensure callbacks are provided when expected by other invoking modules:
```javascript
something.doStuff = function(a, b, callback) {
  if (!(callback instanceof Function)) throw new Error('Expected Callback');
  // ...
  return callback();
};
```

* When an unhandled error occurs, the state of our application is unknown
* The best course of action is to stop accepting new requests, finish serving open requests, and shutdown
* Our cluster will restart the worker after it's shutdown.

```javascript
process.on('uncaughtException', function (err) {
    cluster.worker.disconnect( );
    app.close();
    process.exit(1);
});
```

* Due to the points above, only `throw` when you need to pull down the currently running process.




## Avoid instantiation unless absolutely necessary

* If there is no need to maintain internal state, avoid prototypes.

This is really clunky
```javascript
function DB(url) {
  this.url = url;
}

DB.prototype.info = function (callback) {
  http.get(this.url + '/info', callback);
};

async.parallel([
  function (cb) {
    new DB('http://foo').info(cb);
  },
  function (cb) {
    new DB('http://bar').info(cb);
  }
], ...);
```
This is far nicer
```javascript
var dbInfo = function (url, callback) {
    http.get(url + '/info', callback);
};

async.map(['http://foo', 'http://bar'], dbInfo, ...);
```






## All routes should go through a controller

* Everyone will know where they can start looking for a given request/response
* We can reuse high level functionality by require()'ing other controllers
* We can ensure unit test coverage at the highest level






## Avoid formatting dates wherever possible

* Avoid formatting dates unless absolutely necessary, it's remarkably expensive
* If you can format a date without using `moment`, good
* We should accept and respond with one consistent date format
* If a date needs to be reformatted to please a third party, it should be contained within their module

Bad
```javascript
// This reads "time is a string that looks like HH:mm:ss, reformat it to HH:mm"
return moment(time, 'HH:mm:ss').format('HH:mm')
```
Good
```javascript
return time.substring(0,5);
```




## When writing system and selenium tests, never use static datetime strings

* Time will pass, '2014-12-01' will become history and tests will start breaking

Bad
```javascript
var startDate = '2014-12-01';
```
Good
```javascript
var startDate = moment().add(4, 'weeks');
```




## When writing unit tests, use sinon to fake the time

* As with system tests, you don't want your tests to start breaking after time passes
* Intent is clearer when your tests are asserting against a readable date, rather than the return value of calling something on moment
* You can fast-forward the time to test things you've scheduled to happen later
* You can test otherwise impossible edge cases, like behaviour around leap years

Bad
```javascript
describe('time', function() {
  it('gives you next year', function() {
    assert.equal(library.nextYear(), moment.add('years', 1).format('YYYY-MM-DD'));
  });

  it('does something in 30 seconds, taking ~30 seconds to test', function(done) {
    setTimeout(myTask, 30 * 1000);

    setTimeout(function() {
      assert.ok(myTask.called);
      done();
    }, 31 * 1000);
  });
});
```

Good
```javascript
describe('time', function() {
  var clock;

  beforeEach(function() {
    clock = sinon.useFakeTimers(new Date(2014, 0, 1).valueOf());
  });

  afterEach(function() {
    clock.restore();
  });

  it('gives you one year from Jan 1 2014', function() {
    assert.equal(library.nextYear(), '2015-01-01');
  });

  it('does something in 30 seconds, taking milliseconds to test', function() {
    setTimeout(myTask, 30 * 1000);
    clock.tick(31 * 1000);
    assert.ok(myTask.called);
  });
});
```





## Configuration properties should be within javascript

* Updating a property in an environment variable requires us to kill + restart a NodeJS cluster
* Rolling back a change to a property in an environment variable requires us to kill + restart a NodeJS cluster
* Killing and restarting a NodeJS cluster means removing it from production, which requires infrastructure assistance
* Updating a property in a javascript config file requires nothing more than a deploy
* Rolling back a change to a property in a javascript config file requires nothing more than a deploy

Bad
```
config/something.sh:
export SOME_USERNAME="hapi"

config/config.js:
ourUsername: process.env.SOME_USERNAME,
```
Good
```
config/config.js:
ourUsername: "hapi",
```

# Appendix

## Caveats with testing heavily async code

Consider these functions:
```javascript
createBooking._orchestrate = function(data) {
  return createBooking._sendBook1Request(data)
    .then(createBooking._generatePaymentRequest)
    .then(createBooking._processPayment);
};

createBooking._orchestrate = function(data) {
  async.waterfall([
    function(callback) { return callback(null, data); },
    createBooking._sendBook1Request,
    createBooking._generatePaymentRequest,
    createBooking._processPayment,
  ], callback);
};

```
By looking at this code example, what do we know about `createBooking._generatePaymentRequest`? We know that it must be a function. We can't tell what params it is expecting, nor what data it should return. As far as testing goes, we can only make as assumption that "some data" is passed in, and "other data" comes back out.

If someone were to come in and alter the behaviour of `createBooking._generatePaymentRequest` to make it return something else, there is no clear indication that we're breaking the expectations of `createBooking._processPayment`.

On a side not, it is not possible to get data between `createBooking._sendBook1Request` and `createBooking._processPayment` WITHOUT passing it through `createBooking._generatePaymentRequest`, which shouldn't care about the requirements of the functions around it. Someone could modify `createBooking._generatePaymentRequest` and break the expectations of the other two functions.

We could wrap things up to fix these issues, but it gets pretty nasty:
```javascript
// -- Promise Module --

var createBooking = module.exports = { };

var _ = require('underscore');

createBooking._orchestrate = function(request) {
  var data = {
    request: request
  };
  var bookingRequestParams = _.pick(data, 'request');
  return createBooking._sendBook1Request(bookingRequestParams).then(function(newData) {
      data.bookingResult = newData;
      return data;
    })
    .then(function(data) {
      var paymentRequestParams = _.pick(data, 'request');
      return createBooking._generatePaymentRequest(paymentRequestParams).then(function(newData) {
        data.paymentRequest = newData;
        return data;
      });
    })
    .then(function(data) {
      var processPaymentParams = _.pick(data, 'bookingResult', 'paymentRequest');
      return createBooking._processPayment(processPaymentParams).then(function(newData) {
        data.paymentResponse = newData;
        return data;
      });
    })
};

createBooking._sendBook1Request = function() { };
createBooking._generatePaymentRequest = function() { };
createBooking._processPayment = function() { };

// -- Test --

var sinon = require('sinon');
var assert = require('assert');
require('sinon-bluebird');

sinon.stub(createBooking, '_sendBook1Request').resolves('data1');
sinon.stub(createBooking, '_generatePaymentRequest').resolves('data2');
sinon.stub(createBooking, '_processPayment').resolves('data3');

var data = 'data0';

createBooking._orchestrate(data).then(function(result) {
  assert.deepEqual(result, {
    request: 'data0',
    bookingResult: 'data1',
    paymentRequest: 'data2',
    paymentResponse: 'data3'
  });

  assert.ok(createBooking._sendBook1Request.calledWithPromise({
    request: 'data0'
  }));
  assert.ok(createBooking._generatePaymentRequest.calledWithPromise({
    request: 'data0'
  }));
  assert.ok(createBooking._processPayment.calledWithPromise({
    bookingResult: 'data1',
    paymentRequest: 'data2'
  }));

  sinon.restore(createBooking._sendBook1Request);
  sinon.restore(createBooking._generatePaymentRequest);
  sinon.restore(createBooking._processPayment);
  console.log("Pass");
});
```

The callback variation of the above looks like this:
```javascript
// -- Callback Module --

var createBooking = module.exports = { };

var async = require('async');

createBooking._orchestrate = function(request, callback) {
  async.waterfall([
    function(callback) {
      createBooking._sendBook1Request(request, function(err, book1Response) {
        return callback(err, request, book1Response);
      });
    },
    function(request, book1Response, callback) {
      createBooking._generatePaymentRequest(request, function(err, paymentRequest) {
        return callback(err, request, book1Response, paymentRequest)
      });
    },
    function(request, book1Response, paymentRequest, callback) {
      createBooking._processPayment(book1Response, paymentRequest, function(err, paymentResponse) {
        return callback(err, request, book1Response, paymentRequest, paymentResponse);
      });
    }
  ], callback);
};

createBooking._sendBook1Request = function() { };
createBooking._generatePaymentRequest = function() { };
createBooking._processPayment = function() { };

// -- Test --

var sinon = require('sinon');
var assert = require('assert');
var sandbox = sinon.sandbox.create();

sandbox.stub(createBooking, '_sendBook1Request').yields(null, 'data1');
sandbox.stub(createBooking, '_generatePaymentRequest').yields(null, 'data2');
sandbox.stub(createBooking, '_processPayment').yields(null, 'data3');

createBooking._orchestrate('data0', function(err, request, bookingResult, paymentRequest, paymentResponse) {
  assert.equal(err, null);
  assert.equal(request, 'data0');
  assert.equal(bookingResult, 'data1');
  assert.equal(paymentRequest, 'data2');
  assert.equal(paymentResponse, 'data3');

  assert.ok(createBooking._sendBook1Request.calledWith('data0'));
  assert.ok(createBooking._generatePaymentRequest.calledWith('data0'));
  assert.ok(createBooking._processPayment.calledWith('data1', 'data2'));

  sandbox.restore();
  console.log("Pass");
});
```
