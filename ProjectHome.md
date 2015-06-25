This project provides a version of pytz tuned for Google App Engine.

get gaepytz from [http://pypi.python.org/pypi/gaepytz](http://pypi.python.org/pypi/gaepytz)

### About ###
[pytz](http://pypi.python.org/pypi/pytz/) has a severe performance problem that impedes its usage in App Engine. This is caused because `pytz.__init__` builds a list of available zoneinfos checking the entire zoneinfo database (which means: it tries to open hundreds of files). This is done in the module globals, so it is not easily avoidable. And it is far from ideal to do this in App Engine - app initialization becomes unacceptable if every time we import pytz it checks 500+ files.

In this alternative version, pytz is highly optimized for App Engine, following ideas from [several](http://appengine-cookbook.appspot.com/recipe/caching-pytz-helper/) [recipes](http://takashi-matsuo.blogspot.com/2008/07/using-zipped-pytz-on-gae.html) [around](http://takashi-matsuo.blogspot.com/2008/07/using-newest-zipped-pytz-on-gae.html):

  * database files are not automatically reads when the module is imported
  * the database files are loaded using zipimport to reduce number of files
  * it uses memcache to cache already loaded zoneinfos

This results in almost unnoticeable load time and makes pytz usable in App Engine.

What do you think? Discuss in the [reddit entry](http://www.reddit.com/r/AppEngine/comments/aacnz/pytz_problems_possible_patch/)! :)

### Usage ###

Add pytz to your app directory normally, but import it from the gae module:
```
from pytz.gae import pytz
```
Then monkeypatches will be applied and you can use pytz normally.

### News ###
  * July 12, 2011: Updated zoneinfo to 2011h
  * March 12, 2011: Updated zoneinfo to 2011c
  * February 16, 2011: Updated zoneinfo to 2011b
  * April 20, 2010: Updated zoneinfo to 2010h
  * April 03, 2010: Updated zoneinfo to 2010g
  * Feb.23, 2010:
    * new `TimezoneLoader` class by James Crasta
    * updated zoneinfo to 2010b
  * Nov.20, 2009: initial version