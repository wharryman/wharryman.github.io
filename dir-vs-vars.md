dir(), vars()
=====================
*23 Sep 2013*

Found it useful when trying to do deep-dive debugging, there's some subtle differences in between the two.  

Obligatory links to documentation:
- [python docs for dir()](http://docs.python.org/2/library/functions.html#dir)
- [python docs for vars()](http://docs.python.org/2/library/functions.html#vars)

Taking a look at differences, I was using it to introspect on Q objects.

Here's the output for dir(Q()):

('dir', ['AND', 'OR', '__and__', '__bool__', '__class__', '__contains__', '__deepcopy__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__invert__', '__len__', '__module__', '__new__', '__nonzero__', '__or__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_combine', '_new_instance', 'add', 'children', 'connector', 'default', 'end_subtree', 'negate', 'negated', 'start_subtree', 'subtree_parents'])

Here's the output for vars(Q()):

('vars', {'connector': u'OR', 'negated': False, 'children': [<django.db.models.query_utils.Q object at 0x7fb369056250>, <django.db.models.query_utils.Q object at 0x7fb369056290>], 'subtree_parents': []})

And here's the output for trying to narrow dir() down to attrs (got this from SO, but don't have the link):

```
print('attr', [attr for attr in dir(query) if not attr.startswith('__')])     
```
('attr', ['AND', 'OR', '_combine', '_new_instance', 'add', 'children', 'connector', 'default', 'end_subtree', 'negate', 'negated', 'start_subtree', 'subtree_parents'])

Anyway, just thought I'd capture this for future reference.