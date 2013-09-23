Dynamic Filtering and Q Objects in Django
=========================================
*23 Sep 2013*


Summarizing all of the ways in which I've beat my head against this for the past couple days.  
First off, the main references - read these first, answer's probably in there:

- [Django docs on Q objects](https://docs.djangoproject.com/en/dev/topics/db/queries/#complex-lookups-with-q-objects)
- [The Power of Django's Q Objects](http://www.michelepasin.org/blog/2010/07/20/the-power-of-djangos-q-objects/)
- [Adding Q objects](http://bradmontgomery.blogspot.com/2009/06/adding-q-objects-in-django.html)
- [Q objects and kwargs](http://www.nomadjourney.com/2009/04/dynamic-django-queries-with-kwargs/)
- [SO with clean use of Q objs](http://stackoverflow.com/questions/1166074/a-django-orm-query-using-a-mix-of-filter-and-q-objects)
- [Another SO link on topic](http://stackoverflow.com/questions/852414/how-to-dynamically-compose-an-or-query-filter-in-django)

Now, my issue was the following: I wanted to AND together a group of filter terms, and then OR these groups.  I kept having various troubles getting the correct packing/unpacking right, since it isn't simple X.objects.filter(**kwargs) operation.  I kept getting a Key Error deep inside django's ORM in /sql/query.py, and it took me awhile to realize that I was incorrectly using a dict while forming my Q objects before combining them.  The last reference link there got me to spot it, as in the answer by zzart where he built the Q objects with a tuple.  Time to slap myself on the forehead and review things like args and kwargs.  In any case, I haven't checked this yet, but here's what I got:

```
field_list = []
for key, group in groupby(report.filterfield_set.all(), lambda x: x.grouping):
    field_list.append([x.process_filter() for x in group])
#where process_filter() returns a Q object created from strings, simplifying a bit here - similar to:
#return Q((filter_string+filter_type, filter_value))

query = Q()
for group in field_list:
    q = Q()
    for q_obj in group:
        q.add(q_obj, Q.AND)
    query.add(q, Q.OR)
report_query = model_class.objects.filter(query)
```

I'm sure using functools.reduce or perhaps a comprehension would work as well.  Going to wait to check the SQL first - remember, just print(queryset.query) to see it.

Oh, and reread your args/kwargs:

- [SO overview link](http://stackoverflow.com/questions/287085/what-do-args-and-kwargs-mean)
- [SO kwargs](http://stackoverflow.com/questions/1769403/understanding-kwargs-in-python)
- [SO #3](http://stackoverflow.com/questions/3394835/args-and-kwargs)
- [Agiliq](http://agiliq.com/blog/2012/06/understanding-args-and-kwargs/)
