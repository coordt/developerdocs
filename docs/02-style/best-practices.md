# Best Practices for Python

## Follow the Style Guide

[pep8](https://www.python.org/dev/peps/pep-0008/)

## Anti-Pattern Examples with Corrections

### Not using a list comprehension when appropriate

#### Example 1
``` python
values = [1, 2, 3]

# Bad

doubled_values = []

for x in values:
    doubled_values.append(x*2)


# Still Bad:

doubled_values = list(map(lambda x: x * 2, values))


# Good

doubled_values = [x * 2 for x in values]

# And you can run if for fun to see the time diff
# timeit.timeit("list(map(lambda x: x * 2, l))", setup="l = range(100)")
# timeit.timeit("[x * 2 for x in range(100)]", setup="l = range(100)")
```


#### Example 2
``` python
# Bad

filtered_values = []

for x in values:
    if x < 2:
        filtered_values.append(x*2)

# Bad again

filtered_values = list(filter(lambda x: True if x < 2 else False, values))

# Good

filtered_values = [x for x in values if x < 2]
```


### Looping over a list

``` python
my_favorite_numbers = [1, 2, 3]

# Bad
for i in range(len(my_favorite_numbers)):
    print(my_favorite_numbers[i])


# Good
for number in my_favorite_numbers:
    print(number)
```


### Getting an indexes in a loop

``` python
l = [1, 2, 3]

# Just no
i = 0
for i in l:
    print(i, l[i])
    i += 1


# Bad
for i in range(0, len(l)):
    print(i, l[i])


# Good
for i, x in enumerate(l):
    print(i, x)
```

### Not using .items() to iterate over a list of key/value pairs of a dictionary.

``` python
# Bad
d = {'foo': 1, 'bar': 2}

for key in d:
    print("%s = %d" % (key, d[key]))

# Good

for key, value in d.items():
    print(f"{key} = {value}")

# More readable and a little faster for larger dictionaries
# timeit.timeit("for key in d: (key, d[key])", setup="d = {n: n * 2 for n in range(100)}")
# timeit.timeit("for key, value in d.items(): (key, value)", setup="d = {n: n * 2 for n in range(100)}")
```

### Not using zip() to iterate over a pair of iterables

#### Simple Example
``` python
# Bad

l1 = [1, 2, 3]
l2 = [4, 5, 6]

n = min(len(l1), len(l2))
for i in range(n):
    print(l1[i], l2[i])

# Good

for l1v, l2v in zip(l1, l2):
    print(l1v, l2v)
```

#### Pivot Columns/Rows Example

```python
# From https://stackoverflow.com/a/2429737

rows = ((0, 1), (1, 2), (2, 3), (3, 4), (4, 5))
columns = ((0, 1, 2, 3, 4), (1, 2, 3, 4, 5))
columns == tuple(zip(*rows))
```

#### Construct a Dictionary Example

```python
# https://gist.github.com/JeffPaine/6213790#construct-a-dictionary-from-pairs

keys = ['raymond', 'rachel', 'matthew']
values = ['red', 'green', 'blue']

peoples_favorites_colors = dict(zip(keys, values))
```

### Using "key in list" to check if a key is contained in a list.

``` python
# Bad:

l = [x for x in range(1000)]


if 1000 in l:
    pass


# timeit.timeit('1000 in x', setup='x = [x for x in range(1000)]')

# Good
if 1000 in set(l):
    pass

# timeit.timeit('1000 in x', setup='x = set([x for x in range(1000)])')
```

### Not using 'else' where appropriate in a loop

``` python
# Bad

found = False

l = [1, 2, 3]

for i in l:
    if i == 4:
        found = True
        break

if not found:
    # not found...
    pass

# Good

for i in l:
    if i == 4:
        break
else:
    # not found...
    pass
```

### Not using .setdefault() where appropriate

``` python
# Bad

d = {}

if 'foo' not in d:
    d['foo'] = 1


# Good

d = {}
d.setdefault('foo', 1)
```

### Not using .get() to return a default value from a dict

``` python
# Bad

d = {'foo': 'bar'}

foo = 'default'
if 'foo' in d:
    foo = d['foo']

# Good
d = {'foo': 'bar'}

foo = d.get('foo', 'default')
```

### Not using defaultdict where appropriate

``` python
# Bad
# setting up a bunch of default values in a dictionary

d = {}

if 'x' not in d:
    d['x'] = 0

if 'y' not in d:
    d['y'] = 0

if 'z' not in d:
    d['z'] = 0

# Clever, but still not good
d = {}
for key in ['x', 'y', 'z']:
    d.setdefault(key, 0)


# Good

from collections import defaultdict

d = defaultdict(lambda: 0)

print(d['x'])  # raises no key error
```

### Not using explicit unpacking of sequencing

``` python
# Python supports unpacking of lists, tuples and dicts.
# https://gist.github.com/JeffPaine/6213790#linking-dictionaries


defaults = {'color': 'red', 'user': 'guest', 'log_level': 'info'}
overrides = {'color': 'blue'}
higher_overrides = {'log_level': 'debug'}
highest_overrides = {'color': 'pink', 'user': 'admin'}
most_override_much_wow = {'user': 'superadmin'}

# Bad
# lots of duplicate data
d = defaults.copy()
d.update(overrides)
d.update(higher_overrides)
d.update(highest_overrides)
d.update(most_override_much_wow)


# Good
# one simple call, no duplicate data
from collections import ChainMap

d = ChainMap(
    most_override_much_wow,
    highest_overrides,
    higher_overrides,
    overrides,
    defaults
)

# Best
d = {
    **defaults,
    **overrides,
    **higher_overrides,
    **highest_overrides,
    **most_override_much_wow,
}


```
### Not using explicit unpacking of sequencing

``` python
# Bad

l = [10, -5, 42]

x = l[0]
y = l[1]
z = l[2]

# Wat

x, y, z = l[0], l[1], l[2]

# Good

x, y, z = l
```
### Not using unpacking for updating multiple values at once

``` python

# Bad

x = 1
y = 2

_t = x

x = y + 2
y = x - 4

# Good

x = 1
y = 2

x, y = y + 2, x - 4
```


# Not using 'with' to open files

``` python
# Bad

f = open("file.txt", "r")
content = f.read()
f.close()

# Good

with open("file.txt", "r") as input_file:
    content = f.read()
```

### Asking for permission instead of forgiveness

``` python
# Bad

import os

if os.path.exists("file.txt"):
    os.unlink("file.txt")

# Good

import os

try:
    os.unlink("file.txt")
except OSError:
    pass

# Best
# Python 3.4 or newer
from contextlib import suppress

with suppress(OSError):
    os.remove('somefile.tmp')
```
### Not using a dict comprehension where appropriate

``` python
# Bad

l = [1, 2, 3]

d = dict([(n, n * 2) for n in l])

# Good
# Faster and more readable

d = {n: n * 2 for n in l}

# timeit.timeit("dict([(n, n * 2) for n in l])", setup="l = range(100)")
# timeit.timeit("{n: n * 2 for n in l}", setup="l = range(100)")
```

### Using string concatenation instead of formatting
``` python
# Bad

n_errors = 10

s = "there were " + str(n_errors) + " errors."

# Good

s = "there were %d errors." % n_errors

# Better IMO

s = "there were {0} errors.".format(n_errors)

# Python 3.6+ Only

s = f"there were {n_errors} errors."
```

### Implementing Java-style getters and setters instead of using properties.


``` python
# http://stackoverflow.com/questions/6618002/python-property-versus-getters-and-setters

# Bad

class Foo(object):
    def __init__(self, a):
        self._a = a

    def get_a(self):
        return self._a

    def set_a(self, value):
        self._a = value


# Good

class Foo(object):
    def __init__(self, a):
        self._a = a

    @property
    def a(self):
        return self._a

    @a.setter
    def a(self, value):
        self._a = value


# Best
# Don't over think this; we can add the getters and setters later if need be

class Foo(object):
    def __init__(self, a):
        self.a = a
```

### Implementing a class that has only one function

```python
# Bad

class DateUtil(object):
    @staticmethod
    def from_weekday_to_string(weekday):
        nameds_weekdays = {
            0: 'Monday',
            5: 'Friday'
        }

        return nameds_weekdays[weekday]


# Good

def from_weekday_to_string(weekday):
    nameds_weekdays = {
        0: 'Monday',
        5: 'Friday'
    }

    return nameds_weekdays[weekday]
```