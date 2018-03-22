# Setting up a Python Package for Continuous Testing

When we have good tests for our individual Python packages, we can limit our tests in a project to just the ways the packages interact with each other.

## Tools

These are the tools that we will use to test our Python packages.

- **Jenkins:** Runs the tests automatically, records the results, reports back to GitHub the result
- **Tox:** Runs the tests in multiple environments
- **coverage.py:** Provides test coverage reports
- **pytest:** Testing framework that allows you to write tests as functions
- **python-future:** Tool for creating Python 2 and Python 3 compatible code

## Artifacts

Our tests will generate artifacts that we will use for tracking progress.

- **junit-{envname}.xml:** Provides pass/fail and other information about the tests for Jenkins.
- **coverage.xml:** Provides detailed testing coverage information for Jenkins
- **htmlcov:** Nice HTML version of coverage report
- **term-missing:** Terminal version of the coverage report that also shows which lines are missing test coverage

## The Steps

1. Set up tox on the package
    - An environment added to the tox configuration that means that _we support that configuration from now on._ 
    - A test that fails suddenly on a yet-unused version of Django or Python matters if it is in the tox configuration.
2. Set up the package in Jenkins
    - Even if you only test in one environment while developing, Jenkins ensures that every environment is tested.
