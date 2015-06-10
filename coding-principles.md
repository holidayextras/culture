# Coding Principles
This document is designed for high-level coding guidelines. It needn't go into the specifics of "how many whitespaces to use" or "whether to use single line ifs", but rather should deal with more general things we all agree we like to see in our code.

## Clarity over brevity:
When writing code, we should aim to make it as objectively clear and easy to understand as possible:

* Use descriptive variable and function names.
* Comments should be used when the *why* is unclear, if the *what* is unclear we should look at improving our code clarity.
* Split complex sequences logic out into separate functions.
* Supplement APIs and complex areas of code with markdown documentation in the same repository as the code.

For example, underscore offers a lot of helper functions that can make code more readable. For example, instead of:

```javascript
var myArray = [ 1, 2, 3, 4, 4, 5 ];
var myUniqueArray = [];

myArray.forEach(function(value) {
  if(newArray.indexOf(value) === -1) {
    newArray.push(value)
  }
});
```

We can make this clearer with underscore:

```javascript
var myArray = [ 1, 2, 3, 4, 4, 5 ];
var myUniqueArray = _.uniq(myArray);
```

One might argue that extensive use of this syntactic sugar will lead to a slower codebase, but modern JavaScript engines do a great job of optimising the code we write to the point that the readibility benefits outweigh any minor performance quibbles. In browsers especially, a huge amount more CPU time is lost to rendering than actual application logic.

## Don't reinvent the wheel:
In short, this means that if someone's created a popular, library or framework that solves your problem, you're better off using that than writing your own new code.

There are a few caveats to this:

 * Check the licensing of the library you're using to see if it's legally appropriate for your project.
 * Untested libraries shouldn't be used.
 * Libraries that are not actively maintained shouldn't be used.
 * Pulling in a massive library to use a single function of what it has to offer, especially on the front end.
 
It's tempting and fun to write your own stuff, but we've got more than enough work to get on with writing our code means the following:

 * More code for us to review.
 * More code for us to maintain.
 * More code for us to write tests for.
 * More documentation to write and maintain.
 * It's almost definitely not as good as something with a community of PHDs and Geniuses behind it. *(If it is as good, then you should probably open-source it because people will care.)*
 
Remember, If you need to change the way an open source project you use works, you can always [submit a pull request](https://github.com/hallelujah/valid_email/pull/27).

## Where possible, automate:
Automation allows us to do things quickly, perfectly and often for free. Things like running selenium tests against pull requests automatically and grouping complex sequences of commands behind a single task save us time and give us confidence that things will work correctly in future.

This reduces errors, improves uptime and means we waste less time and money on boring, repetitive and frustrating tasks, let's do as much of this as possible.

## Security:
Security should be considered from the start, this particularly applies to external facing code which interfaces to end users or into other systems (databases, APIs, etc).
The reinventing the wheel argument is also valid here (see above) as any design patterns or technology choices should be "battle tested" in the wild, before being considered.

In this context "battle tested" refers to:

 * Used by other major sites/companies.
 * Have a good history of fixing security bugs in a timely fashion.
 * Have a fairly large community behind the project/idea.
 * Actively maintained.

The document below details the most common security problems affecting web applications, it's worth being mindful of the points made here when writing code:

[OWASP Top Ten 2013](http://owasptop10.googlecode.com/files/OWASP%20Top%2010%20-%202013.pdf)

There are many open source tools and white papers to help understand and test the issues mentioned in the above document. Google is your friend here and searching for terms such as "XSS whitepaper" or "XSS tools" will yield good results.

[Exploit database - Security Papers and Articles](http://www.exploit-db.com/papers/)

[The ultimate XSS protection cheatsheet for developers](http://www.exploit-db.com/wp-content/themes/exploit/docs/33931.pdf)

### Deployment:

Before placing a new system or end point live, the following points should be covered:

 1. "best effort" security tests should be performed by internal staff and results documented.
 1. Add new endpoint/system to PCI and penetration testing hosts list.
 1. Check results of next external security audit for potential issues in new system.

The above would be performed in conjuction with IT and project owners would need to be mindful of the extras time this will incur before go live.

## Linting:

All new projects must have code linting enabled and configured to fail CI if the standard are not meet.

The preferred way of implementing this is to see our [linting rules documentation](https://github.com/holidayextras/culture/javascript-linting-rules.md#adding-to-your-project), as this simplifies the approach provides further consistency among project.
