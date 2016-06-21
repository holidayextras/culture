# HolidayExtras Automated Testing Commandments

## Thou shalt be consistent within projects
If a project has a de-facto standard, it should be maintained. The process for changing a de-facto standard should take the form of a whiteboard meeting, not a pull request.

## Thy tests should be obvious
If the test behaviour isn't obvious by looking at it, it's no good.

## Thy tests should be loosely coupled with thy code
If I make a single code change and many tests fail, it's no good.

## Broken code should break thy tests
If I can delete code and it's corresponding test suite still passes, it's no good.

## Thy broken tests should be helpful
If I break a feature and a test fails, but it isn't obvious what is wrong, it's no good.

## Thy tests should be consistent
If a test inconsistently fails, it's no good.

## Thy tests should be fast
If a unit test takes more than 2 seconds to run, it's no good.
