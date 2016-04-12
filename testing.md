# Testing

## Why write tests?
We write automated tests to allow us to demonstrate that our software meets business requirements, and that it continues to do so as changes are made to the codebase.

## When do I write tests?
Almost all of our code should be tested to some degree, the question is less about "when" and more "to what level do I test this code?". There are a few points to consider to help with this decision making.

### Net costs
It's worth considering the net cost of tests before you write them. Simply put, ask yourself "Is this test likely to pay for itself over the course of it's lifetime?".

If not, it could be that you're incurring a net cost to the business with a over-thorough test that needs to be **1.** understood, **2.** run as part of your CI and **3.** maintained. In this scenario it might be worth opting for a something less expensive that still gives the team an acceptable level of confidence that the code works as expected.

### Keep the system in mind
When making decisions about the scope of your testing, keep the system in mind that you're working on. For example:
 - A single failure on a data-sensitive area of an API is potentially going to affect hundreds of customers and could expose customer data. This needs extensive, thorough, testing to ensure customer data is never exposed.
 - Conversely, s single failure in a simple React component is only going to affect a single session at once, and might just incur a slight visual glitch or formatting issue. We probably want to write a quick test to check this works as expected, but testing every possible edge case is likely to cost us more than the test is worth.
 - Library code should always be tested extensively It's designed to be reused many times across many systems in many use cases - we need to be confident this works as advertised no matter how it's used.

## Types of Testing
There are three major categories of testing that might be appropriate to use in your development.

### Unit Testing
[Unit testing](https://en.wikipedia.org/wiki/Unit_testing) involves testing code at a very granular level, usually testing a single function and everything that goes on within it, stubbing calls to other functions or modules to ensure we can accurately check the functionality of the unit. Taking making a cake as an analogy, you can think of unit testing as checking the quality of the individual ingredients.

Unit tests are particularly useful for things like:
 - Proving the functionality of complex algorithmic logic.
 - Alerting developers to potential regressions during refactors in a "yes, I really did mean to do that there" kind of fashion.
 - Providing 100% thorough exercising of business critical code. E.g. Ensure a card number validator does not block valid cards.
 - Testing potential edge cases for functions. E.g. "What happens when I throw garbage into this?"

However, unit tests have some limitations:
 - Expensive to write
 - Expensive to maintain
 - Only accessible to developers
 - Hard to determine a business purpose from. E.g. "I can see this code is doing this and that, but what does that actually _mean_ in product terms?"
 - Provide no guarantee that when all of your code is put together, that anything actually works as the business expects.

### Integration Testing
[Integration testing](https://en.wikipedia.org/wiki/Integration_testing) involves testing the interaction of multiple units together. It sits "above" unit testing conceptually but below full system testing, and therefore covers a wide range of scopes. We may still stub things in our integration tests, but the purpose of the test is no longer to check that things at the unit level perform as expected, but rather that when put together they do something that makes sense. Taking making a cake as an analogy, you can think of integration testing as checking we get good cake batter when combining the individual ingredients together, or that the resulting sponge has good spongey properties.

Integration tests are particularly useful for:
 - Checking application features work as expected without the overhead of a full system automation. E.g. "Given this model data does our application pass appropriate props to our React components and generate sensible HTML?"
 - Checking more complex regressions that can't be covered by unit tests.
 - Helping to formalise the intended interaction of entities within the system. E.g. "When I trigger a product selected event, is the product added to the basket, does the basket price then update to reflect the change?"

However, integration tests have some limitations:
 - Expensive to write
 - Expensive to maintain
 - Only accessible to developers
 - Dealing with real objects means that test setups can be hard to debug. Not everything is stubbed so finding what's broken a test, checking whether it should or should be stubbed and then addressing the issue can be a time consuming process.
 - They can test the code works as expected, but don't prove the application actually makes sense to the user.
 - Can be prone to slowdowns when left unchecked.
 - Integration tests at the low-level extreme are sometimes better written as unit tests, and at the high-level extreme are sometimes better written as system tests. They can be a tricky judgement call to make sometimes.

### System Testing
[System Testing](https://en.wikipedia.org/wiki/System_testing) involves testing the system as a whole. It sits "above" unit and integration testing conceptually and tests the application by using it the way the end user would use it. The goal with system tests is to make sure the application works as a product and that that it can be shown to perform its business requirements. Taking making a cake as an analogy, you can think of system testing as checking the baked, and iced carrot cake tastes like a good carrot cake.

System tests are particularly useful for:
 - Testing user journeys throughout the application. Proving the application meets business requirements.
 - Making _some_ testing available to non-developers. _Some_ system testing frameworks are human friendly enough that anyone can learn to write good system tests in a reasonable amount of time. This applies mainly to applications with a user interface.
 - Testing very complex regressions that are not practical to unit or integration test.
 - Testing visuals on applications with user interfaces. Some system testing frameworks allow us to generate, store and compare screenshots as well as testing user interface elements for clickability.

However, system tests have some limitations:
 - Slow to run. This can be overcome with parallelism, but that can be expensive and the suite will only ever be as fast as the slowest test.
 - Brittle and prone to false negatives. This can be overcome with data fixtures and awareness of best practices.
 - Loosely constrained. Your application code could be doing some _horrible_ stuff under the hood, but your system test is only concerned with inputs and outputs at the system level and so that probably won't be exposed in your system test.
 
---

Different systems will be suited to a different mix of the above test types. Choose wisely based on what it is you're testing.
