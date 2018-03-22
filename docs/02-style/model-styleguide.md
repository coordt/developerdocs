# Model Styleguide
## Standard imports
Each `models.py` file will need the following (at least) at the top

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.db import models
from django.utils.encoding import python_2_unicode_compatible
from django.utils.translation import ugettext_lazy as _
```

1. Sets the file encoding to `utf-8`
2. Forces all strings to unicode. So `'foo'` is treated as `u'foo'`
3. Empty line to separate imports
4. The models module
5. A decorator to make a model work on both Python 2 and Python 3
6. An internationalization function

## Model markup
The order of model inner classes and standard methods should be as follows (noting that these are not all required):

1. All database fields
2. Custom manager attributes
3. class `Meta`
4. `def __str__()`
5. `def save()`
6. `def get_absolute_url()`
7. Any custom methods

```python
@python_2_unicode_compatible
class EventCategory(models.Model):
    """
    Themes of an event.
    """
    name = models.CharField(
        _('name'),
        max_length=255)
    pricing = models.TextField(_('pricing or dicount info'))
    image = models.ForeignKey(
        'happenings.Image',
        verbose_name=_('image'),
        blank=True,
        null=True)

    class Meta:
        verbose_name = _("Event Category")
        verbose_name_plural = _("Event Categories")
        ordering = ('name', )

    def __str__(self):
        return self.name

	def get_absolute_url(self):
       from django.core.urlresolvers import reverse
       return reverse('happenings.views.category_detail', args=[str(self.id)])

```

If you define a `__str__` method (previously `__unicode__` before Python 3 was supported), decorate the model class with [`python_2_unicode_compatible()`](https://docs.djangoproject.com/en/dev/ref/utils/#django.utils.encoding.python_2_unicode_compatible).

## Lowercase field names
Field names should be all lowercase, using underscores instead of camelCase.

Do this:

```python
class Person(models.Model):
    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)
```

Don’t do this:

```python
class Person(models.Model):
    FirstName = models.CharField(max_length=20)
    Last_Name = models.CharField(max_length=40)
```

## Field paramter indentation

When there is only one parameter, it is acceptable to keep it on the same line as the other code. When there is more than one parameter, indent each parameter on its own line.

Do this:

```python
pricing = models.TextField(_('pricing or dicount info'))
image = models.ForeignKey(
    'happenings.Image',
    verbose_name=_('image'),
    blank=True,
    null=True)
```

Not this:

```python
pricing = models.TextField(_('pricing or dicount info'))
image = models.ForeignKey('happenings.Image', verbose_name=_('image'), blank=True, null=True)
```

## Internationalize strings

The `_` method, an alias of `ugettext_lazy` makes it easy to mark strings for translation.

For more information, read [Django's i18n documentation](https://docs.djangoproject.com/en/dev/topics/i18n/).