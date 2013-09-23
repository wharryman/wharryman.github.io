Drop to pdb
===========
*23 Sep 2013*

A quick way to drop to pdb on exception, if you're not using a 'systemized' version of it (ipdb, pycharm, etc) is the following:

```
try:
    t =child[0].split(LOOKUP_SEP)[0] #shady command goes here
except KeyError: #substitute exception as desired
    import sys, pdb
    pdb.post_mortem(sys.exc_info()[2])
```
[Original SO source](http://stackoverflow.com/questions/7260205/python-exception-keyerror-question)

[PDB docs for reference](http://docs.python.org/library/pdb.html)

Actually haven't used pdb all that much, as the debug output on django is thorough enough that a quick print or two catches the issues.  