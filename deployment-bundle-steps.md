# How to setup a project with deployment to S3

1. Create S3 bucket for production / staging (your-bucket-name-production & your-bucket-name-staging). (If using cloudfront, see [optional settings](#optional-settings))

2. Log into [circleci.com](https://circleci.com/) and add your project. Set up the following credentials: (see other members of the team for these values).
  - Environment Variables
    - GHUSER
    - GHPASS
    - GRAPHITE_API_KEY
    - GRAPHITE_ENDPOINT_PREFIX
    - CLOUDFRONT_DISTRIBUTION_ID (if using cloudfront for your production bundle)
  - AWS CodeDeploy
    - Access Key Id
    - Secret Access Key

3. Create relevant files for your project:
  - [circle.yml](#circleyml)
  - [scripts/deployS3.sh](#deploys3sh)
  - [istanbul.yml](#istanbulyml)

4. Add [scripts](#npm-scripts) to your package.json

5. Add [devDependencies](#devdependencies) to your package.json

6. Make sure your tests & linting pass. Merge to staging, see it deploy!

---

##### Optional Settings

1a. (optional) Setup cloud front on the production bucket & add the following to the end of your `deployS3.sh` script:
```bash
if [ $1 == 'production' ]; then
  aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths /\*
fi
```

---

#### Examples

##### Npm scripts:
```json
"scripts": {
  "build": "webpack --config ./webpack.config.js",
  "start": "WEBPACK_DEV=true webpack-dev-server --config ./webpack.config.js --content-base ./demo/ --inline --progress",
  "lint": "make-up src test -i eslint",
  "ci": "TEST_REPORT_DIRECTORY=${1:-unit}; istanbul cover _mocha --  test/ -R xunit > $CIRCLE_TEST_REPORTS/$TEST_REPORT_DIRECTORY/results.xml",
  "prerelease": "node_modules/@holidayextras/deployment-helpers/nodeApps/preRelease.sh",
  "pretest": "npm run prerelease",
  "test": "npm run lint && npm run test:coverage",
  "test:watch": "mocha --watch",
  "test:coverage": "istanbul cover _mocha",
  "posttest": "istanbul check-coverage",
  "postinstall": "npm run build"
},
```

##### devDependencies:
```json
"@holidayextras/deployment-helpers": "*",
"babel-cli": "^6.10.1",
"babel-core": "^6.0.20",
"babel-eslint": "^7.1.1",
"babel-loader": "^6.0.1",
"babel-preset-es2015": "^6.0.15",
"babel-preset-react": "^6.0.15",
"babel-preset-stage-0": "^6.0.15",
"istanbul": "v1.1.0-alpha.1",
"make-up": "^10.0.0",
"mocha": "^3.2.0",
"webpack": "^1.12.2",
"webpack-dev-server": "^1.12.1"
```

##### istanbul.yml:
```yml
check:
  global:
    statements: 100
    lines: 100
    branches: 100
    functions: 100
```

##### deployS3.sh:
```bash
#!/bin/bash
BUCKET=your-bucket-name-$1
DIR=./dist/

aws s3 sync $DIR s3://$BUCKET --acl public-read --quiet
```

##### circle.yml
```yml
machine:
  node:
    version: 6.9.4
dependencies:
  pre:
    - npm install @holidayextras/deployment-helpers
    - node_modules/@holidayextras/deployment-helpers/nodeApps/preRelease.sh
    - aws configure set default.region eu-west-1
    - aws configure set preview.cloudfront true
test:
  pre:
    - mkdir -p $CIRCLE_TEST_REPORTS/unit
    - mkdir -p $CIRCLE_TEST_REPORTS/coverage
  override:
    - npm run lint
    - npm run ci
  post:
    - npm install coverage-percentage
    - cp -r ./coverage/ $CIRCLE_ARTIFACTS/coverage
    - echo "$GRAPHITE_API_KEY.counters.$CIRCLE_PROJECT_REPONAME.production.test.coverage `./node_modules/.bin/coverage-percentage ./coverage/lcov.info --lcov`" | nc -uw0 carbon.hostedgraphite.com 2003
deployment:
  production:
    branch: master
    commands:
      - npm run build
      - ./scripts/deployS3.sh production
      - node_modules/@holidayextras/deployment-helpers/nodeApps/postRelease.sh production
  staging:
    branch: staging
    commands:
      - npm run build
      - ./scripts/deployS3.sh staging
experimental:
  notify:
    branches:
      only:
        - master
        - staging
```
