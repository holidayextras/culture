# How to setup a project with deployment to S3

1. Create S3 bucket for production / staging (your-bucket-name-production & your-bucket-name-staging). (If using cloudfront, see [optional settings](#optional-settings))

## Using [CircleCi](https://circleci.com/)

1. Log into [circleci.com](https://circleci.com/) and add your project. Set up the following credentials: (see other members of the team for these values).
  - Environment Variables
    - GHUSER
    - GHPASS
    - GRAPHITE_API_KEY
    - GRAPHITE_ENDPOINT_PREFIX
    - CLOUDFRONT_DISTRIBUTION_ID (if using cloudfront for your production bundle)
  - AWS CodeDeploy
    - Access Key Id
    - Secret Access Key

2. Create the circle.yml file (see https://circleci.com/docs/1.0/configuration/ for instructions). An example usage within the business can be seen here: https://github.com/holidayextras/react-bookingengines-care/blob/staging/circle.yml

3. Create a deploy script for the CI tool to move your bundle to an S3 bucket. For example:

  ```bash
  #!/bin/bash
  BUCKET=your-bucket-name-$1
  DIR=./dist/

  aws s3 sync $DIR s3://$BUCKET --acl public-read --quiet
  ```

  If using Cloudfront, you can add the following:

  ```bash
  if [ $1 == 'production' ]; then
    aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths /\*
  fi
  ```
  Example usage in the business: https://github.com/holidayextras/react-bookingengines-care/blob/staging/scripts/deployS3.sh

4. Setup `npm scripts` to ensure your tests pass, builds suceed & then using the circle.yml, call your deploy script to move the bundle from `/dist` directory (assuming this is where your build resides), to S3. Example usage within the business: https://github.com/holidayextras/react-bookingengines-care/blob/staging/package.json#L9-L21
