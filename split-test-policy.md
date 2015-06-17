# Split Testing Policy

Currently we use Abba to perform AB and multi-variate tests across the tripapp. Previously we had to do a lot of existence checking but the new library will improve this.

## Set up

To use the Abba library just require it in.

```javascript
var abba = require('lib/abba');
```

## Declaring a test

Creating a test is done using `abba.test` and supplying a friendly name.

```javascript
abba.test('My test');
```

This can be cached in a `var`. However, Abba caches this itself so for clarity calling `abba.test` is preferable.


## Starting a test

You can create a basic AB test using the `start` method. This will create a control `show_original` and variant `show_alternative`.

```javascript
// start a simple ab
abba.test('My test').start();
```

Though if you require customisation you can set these yourself.

```javascript
// start a simple ab
abba.test('My test')
  .control('control', {weight:3})
  .variant('variant', {weight:1})
  .start();
```

## Using in the app

To use the test within the app the methods are `value`, `is` and `isnt`.

```javascript
// get the variant
var chosen = abba.test('My test').value();

// check the variant, returns boolean
if(abba.test('My test').is('show_original')){
  // original side of split
}
if(abba.test('My test').isnt('show_original')){
  // not original side of split
}
```

## Completing a test

When you want to register the test as complete, most likely to indicate conversion then you can call the `complete` method.

```javascript
// complete the test
abba.test('My test').complete();
```
