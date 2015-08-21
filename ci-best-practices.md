#### CI should...

- only push 1 check to PR's in Github.
- build on clean environments.
- build feature branch `pushes` against any upstream changes in deployable branches (staging/master).
- always use the "best practice project setup" `make` targets.
- notify "some channels" (Slack, email tbc) of a success or failure of a build.
- always keep built production artifacts in S3 for a minimum of X days.
- automatically deploy non feature branches (staging/master).

#### Potential make targets

`make` // project setup, gathering/installing resources

`make test` // runs tests prescribed by the repo (Selenium, unit tests, integration tests etc)

`make deploy` //runs contextually any scripts/tasks associated with pushing code to infrastructure

`make ci` (alias, runs all 3) // used by CI to bootsrap the entire deploy process

