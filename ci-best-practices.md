# CI Best Practices

## CI should...

- Be "as a service".
- Push 1 or more check to PR's in Github.
- Build on clean environments.
- Build feature branch `pushes` against any upstream changes in deployable branches (staging/master).
- Always use the "best practice project setup" `make` targets.
- Notify "some channels" (Slack, email tbc) of a success or failure of a build.
- Always keep built production artifacts in S3 for a minimum of X days.
- Automatically deploy non feature branches (staging/master) when building.
- Always have access to any off box resources it needs (e.g. AWS CLI).
- Be able to run containered tests (tests must be fixtured).
- Not allow sensitive information in scripts/configuration files.
- Be able to run any build on any "agent/minion".
- Allow us to pay for more concurrency.
- Be able to cancel and rerun builds.
- Give confidence that the code is shippable.
- Allow stats to be gathered programmatically around our KPI's.

## Targets

Make is used in the examples below but any method of running tasks can be used as long as the naming convention below is followed.

### Single step CI

`make ci`

This task would install dependencies, run tests and if required begin the entire deploy process.

### Multiple step CI

It is often advantageous to split up a CI's tasks into multiple steps for speed or clarity, as these will often run in parallel.

Make targets should be prefixed with `ci_` then use a descriptive name for the specific task, examples of this would be:

`make ci_lint`
`make cl_selenium`

Both of these tasks will require dependencies installed in order to bring the box into a state where a specific task can be run.

If several tasks require the same initial setup having them split out can make the CI process slower due to an increase job queue.

## Recommended providers

When implementing CI into a project please use one of the following providers (in no order):

* [CircleCi](https://circleci.com/)
    * Offers SSH access into VMs for debugging.
    * Does not test code after merging to master.
    * Slightly faster boot time.

* [Travis Ci](http://docs.travis-ci.com/)
    * Does not offer SSH access
    * Does merge code into master before testing.

There are some slight differences to the providers above as highlighted, but their implementation is very similar, please pick one that suits the project best.

## Implementation

Points to note:

### Do's

* Do create an artifact and promote it toward production (rather than eg re-building the project 3x on integration, staging & production)
* Do cache any dependency directories (such as `node_modules`).
* Do fail the CI process as soon as one task fails.
* Do run the quickest and / or most likely to fail tasks first, to enable a broken build to end quicker.
* If a project has private dependencies, you must do the following:
    * Add the HX Travis / CircleCi GitHub user SSH key to the project config on the travis UI.
    * Give the HX Travis / CircleCi GitHub group access to project and all of its nested dependents.
* Do use scripts if the CI tasks take more than a short line to type / read.
    * With comments, so we know why any strangeness is being performed.
    * Keep with the naming convention above (eg `scripts/ci.sh` or `scripts/ci_lint.sh`).
* Do make sure that the language version specified on CI matches other versions specified elsewhere in the project.
    * Local environments should be as similar to CI / deployment environments.
    * `package.json` / `.nvmrc` / `Gemfile`

### Do not's

* Do not have any secret keys in config files or scripts as mentioned above.
    * Please pass these in via encrypted environment variables, until we have a better solution.
* Do not use excessive multiple environments such as several nodejs versions, this will increase the Ci queue size and may slow down processing of others's builds.
    * Are the extra environments required on every build?
* Do not install global packages on CI
    * Use the language's package management for installing modules locally instead.
    * Set the correct executable path if needed.
* Do not re-purpose the default CI test script for running non test tasks.
    * Use the naming convention specified above instead.

### Travis

1. Enable travis on the project using the travis UI.
1. Create a file named `.travis.yml` in the root of the project.
1. Push code.

#### Example config

```YAML
sudo: false
language: node_js

node_js:
  - 0.10

cache:
  directories:
  - node_modules

script: npm run ci
```

### CircleCi

1. Enable circleCi on the project using the circleCi UI.
1. Create a file named `circle.yml` in the root of the project ([Documentation](https://circleci.com/docs/configuration)).
1. Push code.

#### Example config

```YAML
machine:
  node:
    version: 0.10.38
dependencies:
  cache_directories:
    - node_modules
test:
  override:
    - npm run ci
```
