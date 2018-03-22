# Tests Involving Databases


- If possible, use sqlite as the database.
- If necessary to use PostgreSQL-specific features, such as range fields, you need additional configurations.

## Updating the `tox.ini` file

### Add additional dependencies

Update the `deps` value in the `[testenv]` section to include `psycopg2` and `django-environ`:

```ini
[testenv]
# ...
deps =
    -rrequirements.txt
    pytest
    pytest-cov
    pytest-django
    psycopg2
    django-environ
    django19: Django<1.10
    django110: Django<1.11
    django111: Django<2.0
```

`psycopg2` is required for PostgreSQL. `django-environ` allows us to provide configurations via environment variables and `.env` files.

### Pass in Some Environment Variables

Add the `passenv` configuration to the `[testenv]` section. Have it pass through `TEST_*` and `DJANGO_*` environment variables:

```ini
[testenv]
# ...
passenv = 
    TEST_*
    DJANGO_*
```

This allows Jenkins to set up environment variables and allows tox to pass them through to each testing environment.

### Tell pytest to reuse databases

Update your `[pytest]` section of the `tox.ini` file to include [addopts](https://pytest-django.readthedocs.io/en/latest/database.html#example-work-flow-with-reuse-db-and-create-db). 

```ini
[pytest]
# ...
addopts = --reuse-db
```

This will save some time by not deleting and creating the database for each session. You can force the re-creation of the database by passing in `--create-db` as in `tox -- --create-db`.

## Ignore `.env` files

Add `.env` to your `.gitignore` file. Then you can have local settings for the test database and they won't be accidentally added to the repo.

## Update Django settings to use environment variables

Add these lines to the top of your example project's settings file or to the top of your `conftest.py` file, if you are using that.

```python
import environ


PROJECT_ROOT = environ.Path(__file__) - 2  # two folders back (/project/example/settings.py - 2 = /)
env = environ.Env()

if os.path.exists(PROJECT_ROOT('.env')):
    env.read_env(PROJECT_ROOT('.env'))
```

That sets up Django-Environ and tries to read from a `.env` file.

To configure the database, add these lines to your example project's settings file, or to the top of your `conftest.py` file.

```python
if 'TEST_DATABASE_SERVER' in os.environ:
    TEST_DATABASE_URL = env('TEST_DATABASE_SERVER').rstrip('/') + "/test_<packagename>"
    DATABASES = {
        'default': env.db_url_config(TEST_DATABASE_URL)
    }
    DATABASES['default']['TEST'] = env.db_url_config(TEST_DATABASE_URL)
else:
    DATABASES = {
        'default': env.db('TEST_DATABASE_URL', default="postgresql://postgres:@localhost:5432/test_<packagename>")
    }
    DATABASES['default']['TEST'] = env.db('TEST_DATABASE_URL', default="postgresql://postgres:@localhost:5432/test_<packagename>")
```

This code allows for two methods of specifying the test database: either the full URL for the server and database or just the URL for the server. While developers can pass in the entire URL when running tox locally, we can't do that on the Jenkins server. To make maintenance easier, Jenkins will pass in the `TEST_DATABASE_SERVER` variable using its credential management system. This contains the host, port, username and password for the database server. We need to add on the database name.

If you are using a `conftest.py` file, you will need to pass the `DATABASES` `dict` that you set up to your `settings.configure` function:

```python
def pytest_configure():
    settings.configure(
        DEBUG=True,
        LANGUAGE_CODE='en-us',
        USE_TZ=True,
        DATABASES=DATABASES,
        # ...
    )
```

## Use a `.env` file

Create a file named `.env` and put in local configurations like:

```bash
TEST_DATABASE_URL=postgresql://coolguy21:pazzw0rd@localhost:5432/test_<packagename>
```

[Django-Environ](https://django-environ.readthedocs.io/en/latest/) has other shortcuts for Django settings.

## Tell pytest which tests require DB access

Pytest assumes that no database is needed. You must explicitly tell it that a test or suite of tests require database access.

You can enable it using [pytest marks](http://pytest-django.readthedocs.io/en/latest/database.html#enabling-database-access-in-tests)

```python
import pytest

@pytest.mark.django_db
def test_my_user():
    me = User.objects.get(username='me')
    assert me.is_superuser
```

To specify every test in a Python file:

```python
import pytest

pytestmark = pytest.mark.django_db
```

To specify every test in a `TestCase` class:

```python
import pytest

@pytest.mark.django_db
class TestUsers:
    def test_my_user(self):
        me = User.objects.get(username='me')
        assert me.is_superuser
```
