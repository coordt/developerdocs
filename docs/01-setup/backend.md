# Backend Development Setup

This section will install all the pieces on your machine.

If you prefer to use Vagrant for your development environment, the [Vagrant chapter](./vagrant.md) will help you get set up.

## Install Components

Before you can run anything, there are several components you must install:

- [Homebrew][Homebrew] which makes installing certain pieces on MacOSX easier
- [git][git], for change management
- [pip][pip], for Python package management
- [virtualenv][virtualenv], to isolate the Python development environment
- [virtualenvwrapper][virtualenvwrapper], which provides some commands to make working with virtualenvs easier
- [Redis][Redis], for low-level caching and messaging
- [PostgreSQL][PostgreSQL], for database storage
- [ElasticSearch][ElasticSearch], for local searching

All of these components are installed through your terminal.

[Homebrew]: http://brew.sh/
[git]: https://git-scm.com/
[pip]: https://pip.pypa.io/en/stable/
[virtualenv]: https://virtualenv.pypa.io/en/stable/
[virtualenvwrapper]: https://virtualenvwrapper.readthedocs.io/en/latest/index.html
[Redis]: http://redis.io/
[PostgreSQL]: https://www.postgresql.org/
[ElasticSearch]:  https://www.elastic.co/


### Install Homebrew

At a terminal prompt, execute:

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Install pip

At a terminal prompt, execute:

```bash
$ curl https://bootstrap.pypa.io/get-pip.py | python
```


### Install git

Requires [Homebrew](#install-homebrew).

```bash
$ brew install git
```


### Install Redis

Requires [Homebrew](#install-homebrew).

```bash
$ brew install redis
$ brew services start redis
```


### Install PostgreSQL

Requires [Homebrew](#install-homebrew).

```bash
$ brew install postgres
$ brew services start postgresql
```

### Install ElasticSearch

Requires [Homebrew](#install-homebrew).

```bash
$ brew install elasticsearch
$ brew services start elasticsearch
```


### Install virtualenv

Requires [pip](#install-pip) and `sudo` access.

```bash
$ sudo install virtualenv
```


### Install virtualenvwrapper

Requires [pip](#install-pip) and [virtualenv](#install-virtualenv).

```bash
$ pip install virtualenvwrapper
$ echo "export WORKON_HOME=$HOME/.virtualenvs" >> $HOME/.profile
$ echo "source /usr/local/bin/virtualenvwrapper.sh" >> $HOME/.profile
$ source $HOME/.profile
```


## Clone the Repo

**NOTE** This example assumes that you are storing your work in a `Projects` directory in your home directory. If you are storing your work somewhere else, replace the first step appropriately.

```bash
$ cd ~/Projects
$ git clone git@github.com:exampleorg/coolsite.git
```

### Setup your environment

**NOTE** These examples assumes that you are storing your work in a `Projects` directory in your home directory. If you are storing your work somewhere else, replace those steps appropriately.

### Create a Python virtualenv

Requires [virtualenvwrapper](#install-virtualenvwrapper).

The last line changes the current directory to the project directory as soon as the
virtualenv is activated.

```bash
$ cd ~/Projects/coolsite
$ mkvirtualenv coolsite
$ echo "cd ~/Projects/coolsite" >> ~/.virtualenvs/coolsite/bin/postactivate
```

### Create Local Settings

There are many occasions in which you will want to alter the development configuration of the project.  We don't want those changes to accidentally get committed and sent back to production. So you will make a ``local_settings.py`` file that you can change as you wish. ``git`` ignores this file, so it won't get accidentally committed.

```bash
$ cd ~/Projects/coolsite
$ cp settings/local_settings.py.template settings/local_settings.py
```

After copying the file, open it in your favorite editor and set a few things:

- `DATABASES['default']['USER']` should be your local computer username
- `DATABASES['default']['PASSWORD']` should be your local computer password


### Populate Database

Requires [PostgreSQL](install-postgresql)

The project contains a script to download the latest database backup, drop your
existing database and create a new one with the backup.

```bash
$ cd ~/Projects/coolsite/
$ ./bin/update_local_database
```


### Execute Local Project

```bash
$ cd ~/Projects/coolsite/
$ ./manage.py runserver
```
