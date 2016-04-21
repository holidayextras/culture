# Git Flow and testing/deploy process

This document is an overview of how Git Flow integrates with our testing and deployment procedures. It is not an overview of Git Flow, as there are great examples out there on the web already:
- [Great overview and comparison of Git Flow] (https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
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

The wizard will ask you to choose branches and namespaces to be used by Git Flow. You should be able to accept the defaults (with the exception of the version tag prefix, for which we'll use 'v'), but make sure your choices are as follows for consistency across projects:

```
Branch name for production releases: [master] 
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? [] v
```

## Features

Git Flow features are no different to Git feature branches. The only exception may be that they are always cut from, and merged back in to, the develop branch. 

### Starting a feature

`git flow feature start <feature-name>`

Best practice is to use the JIRA ticket number where available as your feature name.

This command will cut a new branch from the develop branch. You can add the `-F` flag to perform a pull on origin before you start, which is a good way to avoid unnecessary conflicts. Note that you don't need to add the `feature/` prefix, as this is added for you.

If you're collaborating on a feature, or you've finished development, you'll want to publish the feature.

`git flow feature publish <feature-name>`

This simply performs a push, but you don't need to remember the prefix. 

### Testing a feature

Once the feature is in a finished state and pushed to the repo it should be converted in to a pull request using [the usual procedure](pr-best-practices.md).

### Finishing a feature

When a feature is ready to be deployed we want to merge it back in to the develop branch to await the next cut of a release. 

_This is where a project's strategy may vary. For more continual deployment features can be finished when testing is complete. For staged deployments a project may decide to leave the feature in a ready state and only finished when it's to be incorporated in the next sprint._

A feature can either be finished using the command line tools or via GitHub. Both will merge the feature in to the develop branch, but only the command line tool will remove the branch automatically so make sure you remove the branch after merging if doing so via GitHub. Also if the feature is finished on GitHub, or by a colleague, you'll need to remove your local feature branch to keep things tidy.

Usually we would expect this to be done in GitHub once the feedback has been reviewed, but the command line alternative is as follows.

`git flow feature finish <feature-name>`

## Releases

A key part of the Git Flow strategy is release branches, as opposed to merging directly in to master and pulling the branch or a tag. Releases are also branches cut from develop and you can only have one release branch at any given time.

The release branch is tested as _the code_ that will be deployed on to the production server, while being unaffected by changes to the develop branch. This allows development to continue without the risk of accidentally incorporating unintended code. 

### Starting a release

`git flow release start <version-name>`

This will cut a new branch from develop. You do not need to include the `release/` or the version tag prefix. If you choose the semantic release number of `1.0.0` the release branch will be called `release/v1.0.0`.

This command will check whether there is already a release name and make sure there is not already a tag of that name on Master. You'll also be prompted to bump the version number, so you'll want to do this in your package.json or where relevant to your application.

If you're collaborating on a release, or you've finished development, you'll want to publish it just like a feature.

`git flow release publish <version-name>`

### Testing a release

Once a release is ready it should be tested again to make sure no new issues have been introduced. There is currently no review process for releases, but this may be introduced shortly. 

If there are issues with the release, fixes should be committed directly to the release branch.

### Finishing a release

On confirmation the release is ready for deploy it should be completed on the command line. This is because the Git Flow tooling completes multiple steps for us. 

`git flow release finish <version-name>`

This command will merge the release, including any commits directly to it, in to master and back in to develop. It will also tag master with the version name and remove the release branch, opening the door for the next release. 

_Hotfixes aside, this route should be the only way to push code in to Master and it should be an exact replica of the code tested on the staging environment. The only exception may if a hotfix was introduced after the release was cut from develop._

Note that you'll need to run the push commands once the release has been merged and remember to push the tags too.

```
git push origin develop
git push origin master
git push --tags
```

## Hotfixes

Another advantage of maintaining a separate develop branch is that the master branch provides a static, more predictable place for hotfixing the production environment. Hotfixes are the only time in Git Flow you'll be branching from master. 

### Starting a hotfix

`git flow hotfix start <hotfix-name>`

Per releases and features, you don't need to specify the prefix `hotfix/`, so your name may be `v1.0.1-XX-254`, where the JIRA ticket number is available (otherwise something descriptive). 

If you're collaborating on a hotfix, or you've finished development, you'll want to publish it just like a hotfix.

`git flow hotfix publish <hotfix-name>`

### Testing a hotfix

Depending on the level of expedite, a hotfix may or may not need to go through the PR process. 

### Finishing a hotfix

`git flow hotfix finish <hotfix-name>`

This command will merge the hotfix in to master, tag it and merge it back in to develop, as well as remove the hotfix branch.

Note that you'll need to run the push commands once the hotfix has been merged and remember to push the tags too.

```
git push origin develop
git push origin master
git push --tags
```
