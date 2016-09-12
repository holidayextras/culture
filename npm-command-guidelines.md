# NPM Command Structure

##What is this?
This document is an attempt to loosely standardise the names of npm commands we use within our JavaScript projects. This is being proposed to introduce a level of familiarity between projects so that:

 - The learning time common project commands for new developers is shortened.
 - Context switching between projects is less of a headache.
 - Developers make fewer mistakes during project setup.
 - Boilerplate project code becomes more reusable, shortening time to create new projects and encourage similarity between projects at the same time.

This guide has been developed by analysing which npm commands and command patterns are already in use at Holiday Extras, and doing our best to preserve that while establishing a common standard.

##Observations from our repositories
The following observations have been made from analysing the scripts blocks of all of the `package.json` files within the Holiday Extras Github organisation's projects.

###Types of command
Our npm commands tend to be structured into 5 main categories:

 - Build related commands
 - Test related commands
 - Run related commands
 - CI related commands
 - Deploy/Publish related commands

There's also a long tail of miscellaneous commands.

###Command structure
We seem to almost universally use `:` as a separator to namespace our commands. E.g. `build:js` and `build:css`.

###Commands to run at a certain time
Excluding NPM supported hooks (`prestop`, `prestart`, etc), we have a not insignificant number of other commands with names that reflect _when_ tasks should be run. This is in contrast to the majority of our commands have names that reflect _what_ the command does.

##Recommendations
Based on the above, we've come up with the following recommendations for command names.

### Namespace commands
Commands should be namespaced sensibly in a `:` separated format where possible. Common namespaces that we should aim to share between our projects should include `build`, `test`, `start`, `ci`, `deploy`, `watch`, `debug` and `publish`.

###Reserve the top level of a namespace for a 'do-all' command
While we do want to namespace, we don't want to stop people from using the top-level `npm run test` or `npm run build` commands. These should, however, be reserved for a 'do everything' task _if_ one is readily available.

For example, `npm run test` should run _all_ the tests of _all_ types, whereas you may have `npm run test:unit`, `npm run test:integration` and `npm run test:journey` if you wanted to split up your test types into individual commands.

###Keep command names technology agnostic
We shouldn't need to update all of our command names if we decide to switch technologies, and new developers shouldn't have to be familiar with a new technology just to get a project up and running. In support of that, we should use command names that don't reflect the underlying technology where it's not necessary:

**Bad:**

```
npm run karma
```

**Good:**

```
npm run test:browser
```

###Keep commands reflecting the _what_ as well as _when_ where possible
Commands names should succinctly reflect what the command is doing, not just when it should be run. Adding a namespace can help here. For example, instead of `npm run postci`, `npm run postci:clear-cache` might be a better command name that gives more insight as to what's going on. The obvious exception to this is [NPM supported hooks](https://docs.npmjs.com/misc/scripts).
