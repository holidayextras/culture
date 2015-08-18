# Javascript Linting Rules

The following rules should be applied to all of our javascript projects.

If a rule is absent from rule set below, please adhere to the conventions used in the current project.

_A project may use additional rules to ones listed here, but these would not be part of the current standard and such rules are subject to change in the future._

## Current Rules

These are stored in the [.eslintrc](https://raw.githubusercontent.com/holidayextras/culture/master/.eslintrc) in this repository.

For an explanation of these rules please refer to the [ESLint documentation](http://eslint.org/docs/rules/).

### TLDR

For the lazy here is a brief description of the rules decided by SA's:

1. `"no-underscore-dangle": 0`  Allow variables beginning with `_`.
1. `"curly": [2, "multi-line"]` Force curly braces except on single line statements.
1. `"camelcase": [2, {"properties": "never"}]` Force camel case variables except on object properties.
1. `"space-before-blocks": [2, "always"]` Force spaces before starting a block.
1. `"no-extra-parens": [2, "always"]` Don't allow extra redundant brackets in expressions.
1. `"block-scoped-var": 2` Don't allow variables to be defined in a block but used outside of one.
1. `"no-else-return": 2` Don't allow else statements to return.
1. `"no-self-compare": 2` Don't allow a variable to be compared to itself.
1. `"radix": 2` Enforce the specification of the base to use with the `parseInt()` function.
1. `"comma-style": [2, "last"]` Force multi line lists to have the separating comma after each item.
1. `"indent": [2, 2]` Force two space indentation.
1. `"quotes": [2, "single", "avoid-escape"]` Force use of single quotes for strings, except when you need to escape characters.
1. `"linebreak-style": [2, "unix"]` Force UNIX line endings.
1. `"brace-style": [2, "1tbs"]` Force the `one true brace style`.
1. `"no-new-object": 2` Don't allow `new Object()`, use shortened syntax instead.
1. `"no-nested-ternary": 2` Don't allow nested ternary expressions.
1. `"no-unneeded-ternary": 2` Don't allow ternary expressions for just coverting to a boolean.
1. `"object-curly-spacing": [2, "always"]` Force spaces inside curly braces.
1. `"space-after-keywords": [2, "always"]` Force spaces after keywords (if, for, etc).
1. `"space-before-function-paren": [2, "never"]` Don't allow spaces after function keyword.
1. `"valid-jsdoc": 2` Check if JSDoc blocks are valid if used.

Rules which are not in the above list are taken from the eslint recommended rules as of version 0.22.1 (latest at time of conception).

In general the format of the rules specified are:

* A rule name, these may be prefixed with `no-` to not allow something if enabled, otherwise turning on a rule sets a requirement on the code, such as "must use single quotes etc."
* A rule ID, this enable (2) or disables (0) a rule and also sets a warning (1). If a rule is enabled and the code is not compliant eslint with exit with an error.
* Rule options, these vary from rule to rule but generally allow fine tuning of a rule.

## Implementation

### Tech

The rule set is compatible with [ESLint](http://eslint.org/).

#### Comparing changes between eslint versions

The rule set is the basis of the config for eslint, but in each version the config used may differ due to new rules being available or defaults changing.

To do comparison between two rule sets and/or eslint versions follow the steps below:

1. Download the original ruleset as `/tmp/original_rules`
1. Alter the ruleset and save as `/tmp/new_rules`
1. Checkout the `make-up` repo and change into it
1. Run `script/calculate_eslint_config.js /tmp/original_rules > /tmp/old`
1. Change eslint versions if required `npm install eslint@x.x.x`
1. Run `script/calculate_eslint_config.js /tmp/new_rules > /tmp/new`
1. Run `diff /tmp/old /tmp/new` to see actual config changes

### Adding to your project

Rather than adding the ruleset from this repo into your own project, please use [make-up](https://github.com/holidayextras/make-up) to add the checks
to your project as follows:

    npm install make-up --save-dev

Then add a call to `node_modules/.bin/make-up dir1 dir2 ... dirN` in your npm script to perform the checks.

If adding linting to an existing project, it would be advisable to add one directory at a time and resolve errors as they arise.

For more information on this tool please see the [make-up documentation](https://github.com/holidayextras/make-up).

## Expanding current rules

In order for a rule to be added to the above list it requires the following:

1. A valid reason for adoption the rule.
1. Agreement from the SA's.

