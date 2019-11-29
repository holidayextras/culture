# NightwatchJS best practices

## Config

Ensure that your `nightwatch.conf.js` is in the root of your project

## Running all tests

To ensure consistency each project should have the following npm task setup that runs the nightwatch tests:

```shell
npm run test:nightwatch
```

## Running a single test

To run a single test in nightwatch:

```shell
./node_modules/.bin/nightwatch -t <path-to-test>
```

## Writing tests

Every page should have an associated [page object](http://martinfowler.com/bliki/PageObject.html) and each page object should contain elements, sections, selectors and commands associated with that page.

PageObject example waiting for a page to load:

```js
  var myPageObject = module.exports = {};

  myPageObject.commands = [{
      waitForPage: function() {
        return this.waitForElementVisible('@engine', this.api.globals.timeout);
      },
      navigate: function(options) {
        var querystring = qs.stringify(options.params)
        this.api.url(this.url + '/?' + querystring)
        return this
      }
  }];

  myPageObject.url = ‘https://www.holidayextras.co.uk’;

  myPageObject.elements = {
      engine: {
        selector: '.engine'
      }
  };
```

How that would be called in the test:

```js
  'should load the landing page': function(browser) {
      browser.page.landing().navigate().waitForPage();
  }
```

### Page Object Structure

Ideally page objects should contain the following standard functions:

- `waitForPage()` -> waits for an element unique to the page that is defined in the page object elements property.
- `navigate([options])` -> generic function to load the page defined in this page object. Can take an optional `options` param to build up a request or simply direct to a static url. An example of build a dynamic request for the navigate function would be setting an availability url in the future or ensuring that a booking is already made before redirecting to a manage my booking page.
- `continueToX()`-> a standard naming of a function that will navigate to the next page in the flow by interacting with the page, eg clicking a link or submitting a form.

## Conventions

- Use the globals file for storing reusable test data such as product codes and test payment details.
- Keep css selectors out of your tests, instead use elements and sections in your page objects.
- Abstract complex and repeated logic to page commands. However keep this abstraction sensible - do not create commands to simply click a button when the direct interaction is just as simple and easy to understand when it is part of the test.
- Use `waitForElement` methods when waiting to animations or DOM rendering instead of simply pausing the test.
- When logic is shared use configuration passed in from a test to determine what needs to be interacted with, do not use `XifExists` style helpers.
- Share page objects across products and devices where possible - do not unnecessarily duplicate page logic.
- Use nightwatch tests to test user journeys and complex logic involving DOM interactions. Try not to use them for testing static markup that can be better test using other methods.
- Return `this` in your methods to allow method chaining in your tests

## Resources

- [Nightwatch documentation](http://nightwatchjs.org/guide)
- [Great guide on using page objects](http://martinfowler.com/bliki/PageObject.html)
