Aggregation, (Annotate), and ORDER_BY!
======================================
*24 Sep 2013*

Sometimes, Django, ... pow, right in the kisser.

As always, the refs:

- [Django docs: Aggregation](https://docs.djangoproject.com/en/1.2/topics/db/aggregation/#values)
- [SO question on Count with Group By](http://stackoverflow.com/questions/842031/django-equivalent-of-count-with-group-by)

The ability to use Django's ORM to create GROUP_BY queries has been amazingly obscured.  It *is* possible, despite numerous SO answers suggesting that you drop into raw/extra.  The Django way is to use values(), followed by an annotate(), and **oh yeah, make sure there's an order_by() in there**!  I'm almost positive that the docs neglect to mention this.

So, using the docs' example models, if you want to know the count of books in each category, you need to do:
```
Books.objects.values('category__name').order_by('category').annotate(Count('category'))
```
If you leave out the order_by, I imagine that if you have any other ordering (such as alpha by author) applying to the queryset, it'll screw up your ability to get that simple list/dict of:
```
[{'category__name':'Sci-Fi', 'category__count':'981'},...]
```
Another example - you want the average scores for all unique combinations of exams in your CS classes, assuming some sort of generic model like:

```
class Exam
    name = models.Charfield...
    class = models.ForeignKey...
    score = models.Float...
``` 
you get the idea
    
Answer:
```
Exam.objects.values('class__name', 'name').order_by('class__name', 'name).annotate(Avg('score'))
```

You'll get results for every unique combination of class name and exam name.

Btw, you can change the order of that queryset, i.e. Exam.values().annotate().order_by(), and it'll work, but the order_by() has to be there.

Maybe I should change the title here - Aggregation, Annotation, Aggrevation?  Aggrovation?  Order_By(Alliteration)?  Heh.

