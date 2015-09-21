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

## Potential make targets

`make` // project setup, gathering/installing resources

`make test` // runs tests/lint/hint prescribed by the repo (Selenium, unit tests, integration tests etc)

`make deploy` // runs contextually any scripts/tasks associated with pushing code to infrastructure

### Single step CI

`make ci` (alias, may run all 3) // used by CI to bootstrap the entire deploy process

### Multiple step CI

It is often advantageous to split up a CI's tasks into multiple steps for speed or clarity, as these will often run in parallel.

Make targets should be prefixed with `ci_` then use a descriptive name for the specific task, examples of this would be:

`make ci_lint`
`make cl_selenium`

These tasks will include `make` in order to bring the box into a state where a specific task can be run.

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

When setting up CI for a project make sure not to have any secret keys in config files or scripts as mentioned above, instead please pass these in via encrypted environment variables.