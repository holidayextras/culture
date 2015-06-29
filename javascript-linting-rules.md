# Javascript Linting Rules

The following rules should be applied to all of our javascript projects.

_A project may use additional rules to ones listed here, but these would not be part of the current standard and such rules are subject to change in the future._

## Current Rules

These are stored in the [.eslintrc](https://raw.githubusercontent.com/holidayextras/culture/.eslintrc) in this repository.

For an explanation of these rules please refer to the [ESLint documentation](http://eslint.org/docs/rules/).

### TLDR

For the lazy here is a brief description of the rules specified:

1. `"no-underscore-dangle": 0`  Allow variables beginning with `_`.
1. `"curly": [2, "multi-line"]` Force curly braces except on single line statements.
1. `"camelcase": [2, {"properties": "never"}]` Force camel case variables except on object properties.
1. `"space-in-parens": [2, "always"]` Forces spaces inside round brackets and content.
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

The above rules are applied over the top of the eslint defaults.

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
