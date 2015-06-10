# Javascript Linting Rules

The following rules should be applied to all of our javascript projects.

_A project may use additional rules to ones listed here, but these would not be part of the current standard and such rules are subject to change in the future._

## Current Rules

These are stored in the [.eslintrc](https://github.com/holidayextras/culture/.eslintrc) in this repository.

For an explanation of these rules please refer to the [ESLint documentation](http://eslint.org/docs/rules/).

## Implementation

### Tech

The rule set is compatible with [ESLint](http://eslint.org/).

### Adding to your project

Rather than adding the ruleset from this repo into your own project, please use [make-up](https://github.com/holidayextras/make-up) to add the checks
to your project as follows:

    npm install make-up --save-dev

Then add a call to `node_modules/.bin/make-up dir1 dir2 ... dirN` in your npm script to perform the checks.

If adding linting to an existing project, it would be advisable to add one directory at a time and resolve errors as they arise.

## Expanding current rules

In order for a rule to be added to the above list it requires the following:

1. A valid reason for adoption the rule.
1. Agreement from the SA's.
