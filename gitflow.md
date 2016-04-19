# Git Flow and testing/deploy process

This document is an overview of how Git Flow integrates with our testing and deployment procedures. It is not an overview of Git Flow, as there are great examples out there on the web already:
- [Great overview and comparison of Git Flow] (https://www.atlassian.com/git/tutorials/comparing-)
- [Handy, visual Git Flow cheatsheet] (http://danielkummer.github.io/git-flow-cheatsheet/)

As an alternative to branching from and merging directly to the Master branch, Git Flow can bring the following benefits:
- Continual development and integration independent of releases
- Clean hotfixing strategy independent of releases
- Deliberate release strategy
- Clear points for testing engagement and sign off

## Installing Git Flow libraries

There is a Git Flow toolset that makes the adoption and use of the concepts much easier. The rest of this document assumes you have this installed.

Install instructions for Mac (and other OS) can be found here:
https://github.com/nvie/gitflow/wiki/Mac-OS-X

## Initialise GitFlow

To create a new local Git Flow repo, or convert an existing local repo, use the following command:

`git flow init`

The wizard will ask you to choose branches and namespaces to be used by Git Flow. You should be able to accept the defaults, but make sure your choices are as follows for consistency across projects:

```
Branch name for production releases: [master] 
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? []
```

## Features

[Features intro]

### Starting a feature

### Testing a feature

### Finishing a feature

## Releases

[How/when to form a release]

### Starting a release

### Testing a release

### Finishing a release

## Hotfixes

[When to hotfix]

### Starting a hotfix

### Testing a release

### Finishing a release
