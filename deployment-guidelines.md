# Deployment Guidelines

##### Detailed below are general guidelines for production deployments.

1. Deploy little and often, don't let work get held up in staging/integration environments. 
2. We should have the confidence to deploy at any time (our high unit test and Selenium coverage should back this point up).
3. When running a deploy both a **tester**, and a **developer** should be present. This is to provide support to both parties, and puts us in a position to roll-back the changes quickly and effectively if needed.
4. Own your merge - whether you are a tester or developer, see your deployment all the way through to production.
5. Deployments on non-customer facing systems (e.g. HAPI) do not explicitly require a tester to be present. However if the change has the potential to impact the customer facing systems, then this should certainly been considered.
6. Before deploying, ensure you communicate what you are doing:
  - Make sure all deployments to all environments on all repos are communicated in the Deployments chat. 
  - Try to communicate what you are merging as opposed to "I am merging to staging".
  - If your code will remain in a particular environment overnight, communicate with the team when you plan to merge.
  - If your plan is not communicated and blocks someones else's work, your work may be reverted. 
7. When making a change, you should always increment the version according to the guidelines set out in [http://semver.org/](http://semver.org/).

**Obviously the above is a rough guide and *shouldn't* replace common sense. e.g. If something happens out of hours and a developer can't get hold of a tester, they shouldn't hold off deploying a fix.**

N.B. For expedited issues please refer to the following [guide](expedited-procedure.md).

## Web Assets

In order for new code to be deployed to our customers it first needs to be uploaded to the correct S3 bucket.

A single page or app could use code from multiple S3 locations so to help simplify this we have decided on the following structure:

* One S3 bucket per environment, not per project.
* Each project has its own directory inside each environment's bucket.
* Project directories are named the same as the GitHub repository name.

Here is an example of this structure:

    hx-staging-repo
      hx-tracker
      hx-wizard
    hx-production-repo
      hx-tracker
      hx-wizard

Please raise an `INF` JIRA if you require a new S3 directory to be created, once actioned you will be given the correct credentials for CI upload your assets.

### Implementation

We are now using different AWS credentials for our different environments for increased security, however this means that configuration is slightly more complex.

First add your AWS IDs and keys as encrypted environment variables via the Travis or Circle UI (not in the YAML files), the following names should be used here:

* PRODUCTION_AWS_ACCESS_KEY_ID
* PRODUCTION_AWS_SECRET_ACCESS_KEY
* STAGING_AWS_ACCESS_KEY_ID
* STAGING_AWS_SECRET_ACCESS_KEY

Next create a script that uses the [AWS CLI](https://aws.amazon.com/cli/), as this is installed by default on Circle boxes and easily on Travis.

The method we want to use as defined in [Continuous deployment flow](cd-flow.md), is to deploy to a staging environment when pushing to the `staging` branch and deploy to a production environment when pushing to the `master` branch.

The deploy script `scripts/deploy.sh` which using the environment variable `DEPLOY_ENV` will choose the correct S3 bucket to use as this also changes.

The credentials for the script will be taken from the environment variables mentioned above and can be set via the AWS CLI as follows:

    aws configure set aws_access_key_id $PRODUCTION_AWS_ACCESS_KEY_ID
    aws configure set aws_secret_access_key $PRODUCTION_AWS_SECRET_ACCESS_KEY

#### Travis

Example config:

    before_install:
      - pip install --user awscli
      - export PATH=$PATH:$HOME/.local/bin
    deploy:
      provider: script
      script: scripts/deploy.sh
      on:
        branch: staging
        env:
          - DEPLOY_ENV=staging
      on:
        branch: master
        env:
          - DEPLOY_ENV=production

#### Circle CI

Example config:

    deployment:
      production:
        branch: staging
        commands:
          - ./scripts/deploy.sh
        environment
          DEPLOY_ENV: staging
      staging:
        branch: master
        commands:
          - ./scripts/deploy.sh
        environment
          DEPLOY_ENV: production

#### Permissions

Infrastructure will automatically create new buckets with "block all" by default therefore you will also need to set the Access Control List to "public-read", if you're using the CLI just add the following flag `--acl public-read`. The grantee will get `READ` and the owner will only have the access that the keys allow.

##### Local Testing

For testing credentials:

    aws configure
    aws s3 cp some_test_file s3://your_bucket/your_repo_dir/ --region=eu-west-1 

The deploy script can be run as follows:

    STAGING_AWS_ACCESS_KEY_ID=your_id STAGING_AWS_SECRET_ACCESS_KEY=your_key DEPLOY_ENV=staging ./scripts/deploy.sh

## Private NPM Releases

To allow modules and applications to consume our private modules we use git releases named after the version specified in the `package.json` prefixed with a `v` (github convention).

### Implementation

The github release can be created automatically once a merge to the project's master branch has been performed, as shown below.

Please make sure you have included the deployment helper in the project's `devDependencies` as follows:

    "devDependencies": {
      "@holidayextras/deployment-helpers": "^1.4.1"
    }

On CI please set the following environment variables:

    GITHUB_USER
    GITHUB_API_TOKEN

We have specific CircleCI and Travis github user credentials to use here, please ask an SA for details.

#### Travis

Example config:

    deploy:
      provider: script
      script: node_modules/\@holidayextras/deployment-helpers/nodeApps/createPrivateRelease.sh
      skip_cleanup: true
      on:
        branch: master

#### Circle CI

Example config:

    deployment:
      production:
        branch: master
        commands:
          - node_modules/\@holidayextras/deployment-helpers/nodeApps/createPrivateRelease.sh
