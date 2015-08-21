#### CI should...

- be "as a service".
- only push 1 check to PR's in Github.
- build on clean environments.
- build feature branch `pushes` against any upstream changes in deployable branches (staging/master).
- always use the "best practice project setup" `make` targets.
- notify "some channels" (Slack, email tbc) of a success or failure of a build.
- always keep built production artifacts in S3 for a minimum of X days.
- automatically deploy non feature branches (staging/master) when building.
- always have access to any off box resources it needs (e.g. AWS CLI).
- be able to run containered tests (tests must be fixtured).
- not allow sensitive information in scripts/configuration files.
- be able to run any build on any "agent/minion".
- allow us to pay for more concurrency.
- be able to cancel and rerun builds.
- give confidence that the code is shippable.
- allow stats to be gathered programmatically around our KPI's.

#### Potential make targets

`make` // project setup, gathering/installing resources

`make test` // runs tests/lint/hint prescribed by the repo (Selenium, unit tests, integration tests etc)

`make deploy` //runs contextually any scripts/tasks associated with pushing code to infrastructure

`make ci` (alias, runs all 3) // used by CI to bootsrap the entire deploy process

