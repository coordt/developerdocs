# Testing

Branches are tested by creating "test instances" on our test server. Each test
instance is a semi-independent deployment of the project code that allows you and
other testers or decision makers to use the modified version of the web site.

Each instance is "semi-independent" because they initially share the same database.
This is done because database conflicts between instances typically isn't an issue
and the creation, managment and destruction of branch-specific databases is
time intensive. It is possible to provide a test instance with its own test database, just not automatically.

Test instance URLs are at ``http://<branchname>.test.nationalgeographic.org/``

## Commands

Management of the test instances is done though several commands.

- `fab testinstance.create`
- `fab testinstance.update`
- `fab testinstance.remove`
- `fab testinstance.reload`
- `fab testinstance.list`
- `fab testinstance.start`
- `fab testinstance.stop`

## Making a testing instance

To create a new test instance, use the `fab testinstance.create` command, passing in the branch name after a colon, like so:

```bash
$ fab testinstance.create:edu6500
```

The URL of this example test instance would be ``http://edu6500.test.nationalgeographic.org/``.


## Updating a testing instance

If you make changes to your branch, push your changes to GitHub and use the ``fab testinstance.update`` command, passing in the branch name after a colon, like so:

```bash
$ fab testinstance.update:edu6500
```

## Removing a testing instance

Once you are done with a test instance, you can remove it easily with the ``fab testinstance.remove`` command, passing in the branch name after a colon, like so:

```bash
$ fab testinstance.remove:edu6500
```

## Reloading a testing instance

If for some reason you need to reload your test instance, you can do it easily with the ``fab testinstance.reload`` command, passing in the branch name after a colon, like so:

```bash
$ fab testinstance.reload:edu6500
```

## Listing every test instance

To see the test instances currently deployed on the test server, use the ``fab testinstance.list`` command:

```bash
$ fab testinstance.list
```

## Stopping one or every test instance

The ``fab testinstance.stop`` command can stop a single instance if you pass in the branch name:

```bash
$ fab testinstance.stop:edu6500
```

The command without parameters will stop every test instance on the server.

```bash
$ fab testinstance.stop
```

## Starting one or every test instance

The ``fab testinstance.start`` command can start a single instance if you pass in the branch name:

```bash
$ fab testinstance.start:edu6500
```

The command without parameters will start every test instance on the server.

```bash
$ fab testinstance.start
```
