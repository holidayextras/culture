# Javascript Linting Rules

The following rules should be applied to all of our javascript projects.

_A project may use additional rules to ones listed here, but these would not be part of the current standard and such rules are subject to change in the future._

## Current Rules

### Trailing white space

Rule: [no-trailing-spaces](http://eslint.org/docs/rules/no-trailing-spaces)

Extra whitespace changes on lines and show up on code diffs, whitespace changes can be ignored on diffs but its extra work/clicks that we don't need.

This also allows the go to end of line function in your editor to work in a useful way.

This included empty lines.

## Implementation

### Tech

The rule set will be compatible with [ESLint](http://eslint.org/).

### Adding to your project

TODO how to use the rules in your project.

## Expanding current rules

In order for a rule to be added to the above list it requires the following:

1. A corresponding configuration option to enable lint rule.
1. A valid reason for adoption the rule.
1. An example showing code that passes the specific lint check.