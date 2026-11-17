# OWUnit

**OWUnit** is a native OW testing assertion library, designed to work with OW's annotated test runner.

## Requirements

Use the OW interpreter version which supports `@Test`, `@BeforeEach`, `@AfterEach`, `throw:` and `ow test`. These changes are proposed in the companion `ow-lang` pull request. The native LLVM backend does not yet support the test runner.

## Install

Declare the GitHub repository in your project's `dependencies.yml`:

```yaml
format: 1
repositories:
  - id: github
    url: https://github.com
dependencies:
  - name: owunit
    repository: github
    path: OW-Programing-Language/OWUnit
    version: main
```

`version: main` is convenient for development; for production pin a release tag or immutable commit. Then run `ow deps install`.

## Write your first test

```ow
import "@owunit/src/OWAssert.ow"

class CalculatorTest:
  field checker

  @BeforeEach
  fn setup():
    set this.checker: new OWAssert()

  @Test
  fn additionWorks():
    call: this.checker.assertEquals(5, 2 + 3)

  @Test
  fn comparisonWorks():
    call: this.checker.assertTrue(5 > 3)

  @AfterEach
  fn teardown():
    print: "test completed"
```

Run `ow test tests/CalculatorTest.ow` or `ow test tests/`. A test failure produces an error with a nonzero process exit code; other test methods still run.

## Assertions

`OWAssert` supports `assertEquals(expected, actual)`, `assertNotEquals(unexpected, actual)`, `assertTrue(value)`, `assertFalse(value)`, `assertGreaterThan(actual, threshold)`, `assertLessThan(actual, threshold)`, `assertInRange(actual, minimum, maximum)`, `assertArrayLength(items, expected)`, `assertEmpty(items)`, `assertNotEmpty(items)`, `assertContains(items, value)`, `assertNotContains(items, value)`, `assertNull(value)`, `assertNotNull(value)`, and `fail(message)`.

`OWUnit` is implemented in OW, without a Java JUnit runtime dependency. `OWAssert` uses OW's catchable builtin `assert(...)`; annotated test discovery and lifecycle hooks are compiler/interpreter features rather than source-library reflection.

## Local development

```sh
ow test tests/OWAssertTest.ow
ow run examples/example.ow
```

## License

MIT.
