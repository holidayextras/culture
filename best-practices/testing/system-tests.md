# 2.0 System Tests

## Each test should:

- Use product codes that are guaranteed to be available
- Use dynamic dates that are a fixed amount of time in the future
- Not assert against any dynamic data (eg. content) that may change unexpectedly. Instead, fixtures should be used to remove the dependency on any backing services for our tests.

## How many tests should we write?

- Only enough to cover expected customer journeys from start to finish
