# Testing

## Why write tests?

We write automated tests to allow us to quickly demonstrate that our software meeting business requirements. They're one of a few tools we use to increase our confidence in our work prior to deployment.

## When do I write tests?

### Test necessity
_"If this test fails, what business requirement is compromised?"_

This is a good question to ask yourself before writing tests. If the answer is _"None"_, you shouldn't be writing it. If the answer is _"I don't know"_, it's possible the test you're writing has no value.

### Net costs
It's also worth considering the net cost of tests before you write them. For example, if the business impact of a regression is small and unlikely enough, it could be that you're probably incurring a net cost to the business by writing a test that needs to be **1.** understood, **2.** run as part of your CI and **3.** maintained.

### Keep the system in mind
Lastly, when making decisions about the scope of your testing, keep the system in mind that you're working on. For example, a single failure on a data-sensitive area of the API is potentially going to affect hundreds of customers and could expose masses of customer data. Whereas a single failure in React component is only going to affect a single session at once, and probably won't be a showstopper.

## Types of Testing
There are three major categories of testing that might be appropriate to use in your development.

### Unit Testing
[Unit testing](https://en.wikipedia.org/wiki/Unit_testing) involves testing code at a very granular level, usually testing a single function and everything that goes on within it, stubbing calls to other functions or modules to ensure we can accurately check the functionality of the unit. Taking making a cake as an analogy, you can think of unit testing as checking the quality of the individual ingredients.

Unit tests are particularly useful for things like:
 - Proving the functionality of complex algorithmic logic.
 - Alerting developers to potential regressions during refactors in a "yes, I really did mean to do that there" kind of fashion.
 - Providing 100% thorough exercising of business critical code. E.g. Ensure a card number validator does not block valid cards.
 - Testing potential edge cases for functions. E.g. "What happens when I throw garbage into this?"

However, unit tests have some drawbacks:
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

However, integration tests have some drawbacks:
 - Expensive to write
 - Expensive to maintain
 - Dealing with real objects means that test setups can be hard to debug. Not everything is stubbed so finding what's broken a test, checking whether it should or should be stubbed and then addressing the issue can be a time consuming process.
 - They can test the code works as expected, but don't prove the application actually makes sense to the user.
 - Can be prone to slowdowns when left unchecked.

### System Testing
[System Testing](https://en.wikipedia.org/wiki/System_testing) involves testing the system as a whole. It sits "above" unit and integration testing conceptually and tests the application by using it the way the end user would use it. The goal with system tests is to make sure the application works as a product and that that it can be shown to perform its business requirements. Taking making a cake as an analogy, you can think of system testing as checking the baked, and iced carrot cake tastes like a good carrot cake.

System tests are particularly useful for:
 - Testing user journeys throughout the application. Proving the application meets business requirements.
 - Making _some_ testing available to non-developers. _Some_ system testing frameworks are human friendly enough that anyone can learn to write good system tests in a reasonable amount of time. This applies mainly to applications with a user interface.
 - Testing very complex regressions that are not practical to unit or integration test.
 - Testing visuals on applications with user interfaces. Some system testing frameworks allow us to generate, store and compare screenshots as well as testing user interface for clickability.

However, system tests have some drawbacks:
 - Slow to run. This can be overcome with parallelism, but that can be expensive and the suite will only ever be as fast as the slowest test.
 - Brittle and prone to false negatives. This can be overcome with data fixtures and awareness of best practices.
 - Loosely constrained. In laymens terms, your application code could be doing some horrible stuff under the hood, but your system test is only concerned with inputs and outputs at the system level.

---

Different systems will be suited to a different mix of the above test types. Choose wisely based on what it is you're testing.
