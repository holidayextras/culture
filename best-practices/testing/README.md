# Testing at HX

## Related Pages
- [Unit tests](/best-practices/testing/unit-tests.md)
- [System tests](/best-practices/testing/system-tests.md)
- [Nightwatch.js tests](/best-practices/testing/nightwatchjs.md)

## 8 Commandments of Automated Testing

_DISCLAIMER_ - Failure to meet any of these points doesn't mean new ideas or pull requests should be outright rejected. They should be used as points of discussion to ultimately help us ship code faster.

`code` is used in reference to the code being tested. The target, maybe?
`test` is used in reference to the code that is exercising the codebase to validate correctness.

### Thou shalt be consistent within projects

If a project has a de-facto standard, it should be maintained. The process for changing a de-facto standard should be people focused, and should not be initially communicated via a pull request.

### Thy tests should be obvious

If the test behaviour isn't obvious by looking at it, it's no good. If I have to scroll for days to read a test, it's no good.

### Thy tests should only assert for something once

If I make a single code change and multiple tests fail, it's no good.

### Broken code should break thy tests

If I can delete code that is needed and it's corresponding test suite still passes, it's no good.

### Thy broken tests should be helpful

If I break a feature and a test fails, but it isn't obvious what is wrong, it's no good.

### Thy tests should be consistent

If a test inconsistently fails, it's no good.

### Thy tests should be fast

If a single test takes more than 2 seconds to run, it's no good. Exceptions can be allowed for Selenium.

### Thy tests should be described via emergent properties

If a test can't be described by the impact it has on the consumer of the code, it probably doesn't warrant a test.
