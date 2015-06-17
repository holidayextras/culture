
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






## Handling heavily sequential async code

* `async.waterfall` allows us to chain functions together
* The closures around `_task1/2/3/4` allow us to pass data over each private function instead of needing to proxy data through functions.
* One unit test can efficiently validate the entire code block

```javascript
async.waterfall([
  // ... ,
  function sendBook1Request(prebookingResponse, callback) {
    createBooking._sendBook1Request(prebookingResponse, params, supplier, function(err, book1Response) {
      return callback(err, book1Response);
    });
  },
  function generatePaymentRequest(book1Response, callback) {
    createBooking._generatePaymentRequest(params, payment, function(err, paymentRequest) {
      return callback(err, book1Response, paymentRequest)
    });
  },
  function processPayment(book1Response, paymentRequest, callback) {
    createBooking._processPayment(paymentRequest, payment, function(err, bookingResponse) {
      return callback(err, book1Response, paymentRequest, bookingResponse);
    });
  },
  function sendBook2Request(book1Response, paymentRequest, bookingResponse, callback) {
    createBooking._sendBook2Request(params, book1Response, paymentRequest, bookingResponse, supplier, payment, function(err, book2Response) {
      return callback(err, bookingResponse, book2Response);
    });
  },
  function generateBookingReference(bookingResponse, book2Response, callback) {
    createBooking._generateBookingReference(self, bookingResponse, book2Response, callback);
  },
], callback);
```
```javascript
var sandbox = sinon.sandbox.create();
sandbox.stub(createBooking, '_sendBook1Request').callsArgWith(3, null, 'book1Response');
sandbox.stub(createBooking, '_generatePaymentRequest').callsArgWith(2, null, 'paymentRequest');
sandbox.stub(createBooking, '_processPayment').callsArgWith(2, null, 'bookingResponse');
sandbox.stub(createBooking, '_sendBook2Request').callsArgWith(6, null, 'book2Response');
sandbox.stub(createBooking, '_generateBookingReference').callsArgWith(3, null, 'AAbookingRef');

var product = { _product: { } };
var params = { booking: { params: { } } };
var self = { se:'lf' };

createBooking.call(self, product, params, 'upgrades', 'supplier', 'payment', function(err, result) {
  assert.equal(err, null);
  assert.ok(result, 'AAbookingRef');

  assert.ok(createBooking._sendBook1Request.calledWith('lastResponse', params, 'supplier'));
  assert.ok(createBooking._generatePaymentRequest.calledWith(params, 'payment'));
  assert.ok(createBooking._processPayment.calledWith('paymentRequest', 'payment'));
  assert.ok(createBooking._sendBook2Request.calledWith(params, 'book1Response', 'paymentRequest', 'bookingResponse', 'supplier', 'payment'));
  assert.ok(createBooking._generateBookingReference.calledWith(self, 'bookingResponse', 'book2Response'));

  sandbox.restore();
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