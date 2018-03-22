# Setting up tox

## Initial setup

- Tox should at least test the version of Python and Django we are currently using.
- Initially, the package must contain at least one test that passes, even if it is a "fake" test.

## Ignore some stuff

Add some lines to your `.gitignore`:

```
.tox/
.cache/
.coverage
coverage.xml
htmlcov/
junit*.xml
```

We don't need any of the tox environments or reports to get committed to the repo.

## Initial Configuration

Initially we will set up tox to only test one version of Python and Django. As we write tests, we can expand that to multiple versions.

Here is an initial configuration that will create two environments (replace `<packagename>` with the name of your package):

- **py27-lint:** Uses `flake8` to lint all the code 
- **py{27}-django{19}:** Uses pytest to run tests using an example Django configuration and generate the reports that we need. The `{}` allow us to easily add additional variations. For example `py{27,36}-django{19,110,111}` would test every combination of Python 2.7 and 3.6 and Django 1.9, 1.10 and 1.11.

```ini
[tox]
skip_missing_interpreters = True
envlist =
    py27-lint
    py{27}-django{19}

[testenv]
install_command = pip install --extra-index-url https://pypi.nationalgeographic.org/simple/ {opts} {packages}
commands = pytest --cov=<packagename> \
            --cov-report term-missing \
            --cov-report html \
            --cov-report xml \
            --junitxml=junit-{envname}.xml \
            --ds=example.settings {posargs}
deps =
    -rrequirements.txt
    pytest
    pytest-cov
    pytest-django
    django19: Django<1.10
    django110: Django<1.11
    django111: Django<2.0
setenv =
    PYTHONPATH={toxinidir}:{toxinidir}/example

[testenv:py27-lint]
deps=
    flake8

commands=
    flake8 .

[pytest]
python_files = tests.py **/tests.py **/tests/*.py

[coverage:run]
omit =
    */tests.py
    */migrations/*
    */settings.py
    <packagename>/__init__.py

[flake8]
exclude =
    .git
    __pycache__
    doc_src
    build
    dist
    .tox
    example
    tests
    tests*.py
ignore = E124,E127,E128,E305,W601,E501
max-line-length = 119
```


1. The `[tox]` [section](https://tox.readthedocs.io/en/latest/config.html#tox-global-settings) is the global tox configuration.
    - `envlist` defines the environments for testing. Each environment can have its own section for configuration.
    - `skip_missing_interpreters`  will force tox to return success even if some of the specified environments were missing. This is useful for Jenkins or running on a developer box, where you might only have a subset of all your supported interpreters installed but don’t want to mark the build as failed because of it.
2. The `[testenv]` [section](https://tox.readthedocs.io/en/latest/config.html#virtualenv-test-environment-settings) is the defaults for all test environments.
    - `install_command` sets the command to use to install the dependencies. It adds our PyPI server into list of places to look for packages.
    - The `deps` section includes the `requirements.txt`, pytest, `pytest-cov` for the `coverage` plugin, `pytest-django` for the Django plugin
        - The strange `django19:`, `django110:`, `django11:` lines are [conditional dependencies](https://tox.readthedocs.io/en/latest/config.html#generating-environments-conditional-settings). They are only installed if the environment matches that part.
    - `setenv` [sets environment variables](https://tox.readthedocs.io/en/latest/config.html#confval-setenv=MULTI-LINE-LIST).
    - `commands` [the commands to call for testing](https://tox.readthedocs.io/en/latest/config.html#confval-commands=ARGVLIST)
        - pytest is used to run the testing suite
        - `--cov` in the command tells the `coverage` plugin to only provide coverage for the `<packagename>`
        - `--cov-report term-missing` in the command tells the `coverage` plugin to provide a report of the missing lines to the terminal
        - `--cov-report html` in the command tells the `coverage` plugin to also provide the HTML version of the coverage results. This is used in by developers and Jenkins.
        - `--cov-report xml` in the command tells the `coverage` plugin to also provide a `coverage.xml` file. This is used in Jenkins.
        - `--junitxml=junit-{envname}.xml` tells pytest to write out a `junit`-compatible file showing test results for each environment. This is used in Jenkins.
        - `--ds=example.settings` tells the pytest Django plugin to use `example.settings` as the Django settings module
3. The `[testenv:py27-lint]` section is the custom configuration for the `py27-lint` environment.
    - It only depends on `flake8` being installed
    - It runs `flake8 .` instead of the default testing commands
4. The `[pytest]` [section](https://docs.pytest.org/en/latest/customize.html#builtin-configuration-file-options) provides configuration options for pytest
    - The `python_files` option tells pytest where to look for tests
5. The `[coverage:run]` section provides [configuration](https://coverage.readthedocs.io/en/coverage-4.4.1/config.html#run) for coverage.py.
    - `omit` declares a list of file name patterns to leave out of measurement or reporting.
6. The `[flake8]` section provides configuration for the [flake8 tool](http://flake8.pycqa.org/en/latest/user/options.html#options-list).
    - `exclude` defines a list of glob patterns to exclude from checks.
    - `ignore` defines a list of codes to ignore.
    - `max-line-length` defines the maximum length that any line can be.

## Testing without a project
### Creating a `conftest.py` file

If you don't want to create an example project to run tests or experiment with your package, you can create a `conftest.py` file in the repository directory that sets up Django and runs the tests.

Here is an example `conftest.py` file:

```python
from django.conf import settings


def pytest_configure():
    settings.configure(
        DEBUG=True,
        LANGUAGE_CODE='en-us',
        USE_TZ=True,
        DATABASES={
            'default': {
                'ENGINE': 'django.db.backends.sqlite3',
                'NAME': 'dev.db',
            }
        },
        ROOT_URLCONF='<packagename>.urls',
        INSTALLED_APPS=[
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sites',
            '<packagename>',
        ],
        SITE_ID=1,
        TEMPLATES=[{
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    "django.contrib.auth.context_processors.auth",
                    "django.template.context_processors.debug",
                    "django.template.context_processors.i18n",
                    "django.template.context_processors.media",
                    "django.template.context_processors.static",
                    "django.template.context_processors.tz",
                    "django.template.context_processors.request",
                    "django.contrib.messages.context_processors.messages",
                ],
            },
        }, ]
    )
```

### Alter your testing commands

The `[testenv]` `commands` section, you no longer need the `--ds=example.settings` option, so you can remove it:

```ini
[testenv]
# ...
commands = pytest --cov=<packagename> \
            --cov-report term-missing \
            --cov-report html \
            --cov-report xml \
            --junitxml=junit-{envname}.xml \
            {posargs}
```

