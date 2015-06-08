# Javascript Linting Rules

The following rules should be applied to all of our javascript projects.

_A project may use additional rules to ones listed here, but these would not be part of the current standard and such rules are subject to change in the future._

## Current Rules

These are stored in the [.eslintrc](https://github.com/holidayextras/culture/blob/linting/.eslintrc) in this repository.

For an explanation of these rules please refer to the [ESLint documentation](http://eslint.org/docs/rules/).

## Implementation

### Tech

The rule set is compatible with [ESLint](http://eslint.org/).

### Adding to your project

Rather than adding the ruleset from this repo into your own project, please use [make-up](https://github.com/holidayextras/make-up/tree/check) to add the checks
to your project as follows:

    npm install make-up --save-dev

Then add a call to `node_modules/.bin/make-up` in your npm script to perform the checks.

## Expanding current rules

In order for a rule to be added to the above list it requires the following:

1. A valid reason for adoption the rule.
1. An example showing code that passes the specific lint check.