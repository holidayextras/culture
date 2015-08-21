
CI should only push 1 check to PR's in Github.
CI builds should be on clean environments.
CI should build feature branch `pushes` against any upstream changes in deployable branches (staging/master).
CI will always use the "best practice project setup" `make` targets.
CI should notify "some channels" (Slack, email tbc) of a success or failure of a build.
CI should always keep built production artifacts in S3 for a minimum of X days.
CI should automatically deploy non feature branches (staging/master).


`make` // project setup, gathering/installing resources
`make test` // runs tests prescribed by the repo (Selenium, unit tests, integration tests etc)
`make deploy` //runs contextually any scripts/tasks associated with pushing code to infrastructure
`make ci` (alias, runs all 3) // used by CI to bootsrap the entire deploy process

