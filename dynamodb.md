# AWS DynamoDB

## Running locally

DynamoDB can be run [locally](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) for development and testing purposes.

## Caveats

### Global name space

Each of the environments has a global name space which means tables need to be prefixed accordingly.

_(TODO: agree and document a convention for name spacing)_

### Verbosity

Direct access to DynamoDB can be very verbose an require a lot of boilerplate code. A library to abstract the interactions with the data store may help. Some suggestions:

- [`dynamoose`](https://github.com/automategreen/dynamoose)
- [`dyno`](https://github.com/mapbox/dyno)

### Provisioned throughput

DynamoDB has minimum and maximum limits [provisioning] (http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html#limits-provisioned-throughput-min-max) for read and write throughput. These default to low values and will probably need to be adjusted for the needs of your application.
