# NightwatchJS best practices  #
----------

## Running tests ##

----------

To ensure consistency each project should have the following npm task setup that runs the nightwatch tests:

    npm run test:nightwatch


**Running a specific test**
To run a single test in nightwatch

    ./node_modules/.bin/nightwatch -c <path-to-config> -t <path-to-test>

## Writing tests ##


----------


Every page should have an associated [page object](http://martinfowler.com/bliki/PageObject.html) and each page object should contain elements, sections, selectors and commands associated with that page.

PageObject example waiting for a page to load:

    ‘use strict’;
    var myPageObject = module.exports = {};
    
    myPageObject.commands = [{
        waitForPage: function() {
          return this.waitForElementVisible('@engine', this.api.globals.timeout);
        }
    }];
    
    myPageObject.url = ‘https://www.essentialtravel.co.uk/essential-travel-ins.html’;
    
    myPageObject.elements = {
        engine: {
          selector: '.engine'
        }
    };

How that would be called in the test:

    'should load the landing page': function(browser) {
        browser.page.essentialLanding().navigate().waitForPage();
    }

## Do's and Don'ts ##


----------


**Do’s**

- Use the globals file for storing reusable test data such as product codes and test payment details.
- Keep css selectors out of your tests. Use page objects and page commands instead
- Use commands where possible to avoid code duplication and app specific logic in your tests

**Don'ts**

- Put functions in your globals file, instead create them as a custom command
- Use browser.pause, wait for element instead where possible

## Resources ##
- [Nightwatch documentation] (http://nightwatchjs.org/guide)
- [Great guide on using page objects] (http://martinfowler.com/bliki/PageObject.html)


