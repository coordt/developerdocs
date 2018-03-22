# Python Package Index (PyPI)


# Setting up on your computer

Add the following to your `.pypyrc` file in your home directory.

```ini
[coolorg]
username=coolorgdev
password=TopSeeecret
```

The following is a full example of a ~/.pypirc file.


```ini
[distutils]
index-servers =
  pypi
  coolorg

[pypi]
repository=https://pypi.python.org/pypi/
username=my_username
password=my_password

[coolorg]
repository:https://pypi.coolorg.org/simple/
username=coolorgdev
password=TopSeeecret
```

# Submitting packages

Apps are uploaded uploaded to GitHub and PyPI. After your `~/
.pypirc` file is properly configured you can upload the project to the NatGeo
PyPI with the following snippet.


```bash
$ python setup.py bdist_wheel upload -r coolorg
```
