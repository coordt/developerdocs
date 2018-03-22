# Coding Style

## Editor Configuration

Please conform to the indentation style dictated in the `.editorconfig` file. We recommend using a text editor with [EditorConfig](http://editorconfig.org/) support to avoid indentation and whitespace issues. 

## Python
Unless otherwise specified, follow [PEP 8](https://www.python.org/dev/peps/pep-0008). Also, refer to [*The Hitchhiker's Guide to Python*](http://docs.python-guide.org/en/latest/writing/style/) for good tips to writing idomatic Python.

Use [flake8](https://pypi.python.org/pypi/flake8) to check for problems in this area. Note that our `setup.cfg` file contains some excluded files as well as some excluded errors that we don’t consider as gross violations. Remember that [PEP 8](https://www.python.org/dev/peps/pep-0008) is only a guide, so respect the style of the surrounding code as a primary goal.

An exception to [PEP 8](https://www.python.org/dev/peps/pep-0008) is our rules on line lengths. Don’t limit lines of code to 79 characters if it means the code looks significantly uglier or is harder to read. We allow up to 119 characters as this is the width of GitHub code review; anything longer requires horizontal scrolling which makes review more difficult. This check is included when you run `flake8`. Documentation, comments, and docstrings should be wrapped at 79 characters, even though [PEP 8](https://www.python.org/dev/peps/pep-0008) suggests 72.

### flake8 and editors

Sublime Text

- [Flake8Lint](https://github.com/dreadatour/Flake8Lint)

Vim

- [vim-flake8](https://github.com/nvie/vim-flake8)

Atom

- [linter-flake8](https://atom.io/packages/linter-flake8)

Emacs

- [flycheck](http://www.flycheck.org/en/latest/)

### flake8 and "I meant to do that!"
If `flake8` raises a warning for something you intended to do, add `# NOQA` at the end of the line to silence the warning.

### Basic Guidelines

- Use four spaces for indentation.
- Use underscores, not camelCase, for variable, function and method names (i.e. `poll.get_unique_voters()`, not `poll.getUniqueVoters()`).
- Use `InitialCaps` for class names (or for factory functions that return classes).
- In docstrings, follow [PEP 257](https://www.python.org/dev/peps/pep-0257). For example:

	```python
	def foo():
	    """
	    Calculate something and return the result.
	    """
	    ...
	```
- Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.

	Imports should be grouped in the following order:

	1. standard library imports
	1. related third party imports
	1. local application/library specific imports

	You should put a blank line between each group of imports.

- Imports should usually be on separate lines, e.g.:  
	Yes:
	
	```python
	import os
	import sys
	```

	No:

	```python
	import sys, os
	```

	It's okay to say this though:

	```python
	from subprocess import Popen, PIPE
	```
	
	For long imports, enclose the module list in parentheses
	
	```python
	from .models import (Event, EventCategory, EventRestriction, EventAudience,
        EventInterest, EventDate, EventPrice, EventRelation, Entity, Place)
   ```
   
- Do not put extra whitespace immediately inside parentheses, brackets or braces.

	Yes: `spam(ham[1], {eggs: 2})`
	
	No: `spam( ham[ 1 ], { eggs: 2 } )`

- Do not put more than one space around an assignment (or other) operator to align it with another.

	Yes:

	```python
	x = 1
	y = 2
	long_variable = 3
	```

	No:

	```python
	x             = 1
	y             = 2
	long_variable = 3
	```

## Templates

In Django template code, put one (and only one) space between the curly brackets and the tag contents.

Do this:

```
{% block foo_printer %}
	{{ foo }}
{% endblock foot_printer %}
```

Don’t do this:

```
{%block foo_printer%}
{{foo}}
{%endblock foo_printer%}
```

Name your closing block tags to make it clear which block it is closing.

Do this:

```
{% block foo_printer %}
	{{ foo }}
	{% block bar %}
		{{ bar }}
	{% endblock bar %}
{% endblock foot_printer %}
```

Don’t do this:

```
{%block foo_printer%}
{{foo}}
{%block bar%}{{bar}}
{%endblock%}
{%endblock%}
```
