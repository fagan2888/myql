mYQL
=========

[![Build Status](https://travis-ci.org/josuebrunel/myql.svg?branch=master)](https://travis-ci.org/josuebrunel/myql) [![Documentation Status](https://readthedocs.org/projects/myql/badge/?version=latest)](https://myql.readthedocs.org)
[![Latest Version](https://pypip.in/version/myql/badge.svg)](https://pypi.python.org/pypi/myql/)
[![Downloads](https://pypip.in/download/myql/badge.svg)](https://pypi.python.org/pypi/myql) 
[![Py_Versions](https://pypip.in/py_versions/myql/badge.svg)](https://pypi.python.org/pypi/myql)
[![Implementations](https://pypip.in/implementation/myql/badge.svg)](https://pypi.python.org/pypi/myql)
[![Join the chat at https://gitter.im/josuebrunel/myql](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/josuebrunel/myql?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Code Issues](https://www.quantifiedcode.com/project/gh:josuebrunel:myql/badge.svg)](https://www.quantifiedcode.com/app/project/gh:josuebrunel:myql)


mYQL is a Python wrapper of the Yahoo Query Language.

Yahoo! Query Language Documentation and Support
===============================================

* Yahoo! Query Language - http://developer.yahoo.com/yql/
* Yahoo! Developer Network: http://developer.yahoo.com
* Yahoo! Application Platform - http://developer.yahoo.com/yap/
* Yahoo! Social APIs - http://developer.yahoo.com/social/
* Yahoo! QUery Language Console https://developer.yahoo.com/yql/console/


Release Notes
==============

v 1.2.2 ( development )
-------
* Python3 support OK [#71](https://github.com/josuebrunel/myql/issues/71)
* Fixed issue with **IN** condition in **where** clause
* Fixed issue when passing an empty list/tuple (**[]/()**) in a **where** clause
* Import of StockParser from Gurchet Rai https://github.com/gurch101/StockScraper OK [#68](https://github.com/josuebrunel/myql/issues/68)
* Insert, Update, Delete methods added [#67](https://github.com/josuebrunel/myql/issues/67) 
* Dummy *try/except* removed from main module
* Fixed **Invalid OAuth Signature** when using a refreshed token [#64](https://github.com/josuebrunel/myql/issues/64)
* Fixed misused of ***MYQL.use(...)*** [#76](https://github.com/josuebrunel/myql/issues/76)
* Fixed format issue [#82](https://github.com/josuebrunel/myql/issues/82)
* Added useful functions utils [#81](https://github.com/josuebrunel/myql/issues/81)

v 1.2.1
------
* Multiple requests while using OAuth fixed

v 1.2.0
-------
* OpenTable classes
* Access to resources requiring authentication

v 0.5.6
-------------
* fetch data
* access to community data
* select data format (xml/json)
* change data source
* filter data 
* fix handling of default and on the fly response format
* fix limit on ***select(...).where(...)*** when no limit value is passed
* fix limit on ***get(...)***

installation
============

```shell
$ pip install myql
```

how to use
==========

```python
>>> import myql
>>> yql = myql.MYQL()
>>> yql.diagnostics = True # To turn diagnostics on
```

####access to community tables

```python
>>> yql = myql.MYQL()
>>> rep = yql.rawQuery('desc yahoo.finance.quotes ')
>>> rep.json()
{u'error': {u'lang': u'en-US', u'description': u'No definition found for Table yahoo.finance.quotes'}}
>>> yql.community= True # Setting up access to community
>>> rep = yql.rawQuery('desc yahoo.finance.quotes ')
>>> rep.json()
{u'query': {u'count': 1, u'lang': u'en-US', u'results': {u'table': {u'src': u'http://www.datatables.org/yahoo/finance/yahoo.finance.quotes.xml', u'hash': u'061616a1c033ae89aaf2cbe83790b979', u'name': u'yahoo.finance.quotes', u'request': {u'select': {u'key': {u'required': u'true', u'type': u'xs:string', u'name': u'symbol'}}}, u'meta': {u'sampleQuery': u'\n\t\t\tselect * from yahoo.finance.quotes where symbol in ("YHOO","AAPL","GOOG","MSFT")\n\t\t'}, u'security': u'ANY'}}, u'created': u'2014-08-24T11:26:48Z'}}
>>>
```

***OR***

```python
>>> import myql
>>> yql = myql.MYQL(community=True)
>>> # do your magic 
```

####changing response format (xml or json)

The response format is by default ***json***.

```python
>>> import myql
>>> yql = myql.MYQL(format='xml', community=True)
>>> rep = yql.rawQuery('select name, woeid from geo.states where place="Congo"')
>>> rep.text
u'<?xml version="1.0" encoding="UTF-8"?>\n<query xmlns:yahoo="http://www.yahooapis.com/v1/base.rng" yahoo:count="11" yahoo:created="2014-08-27T04:52:22Z" yahoo:lang="en-US"><results><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Cuvette-Ouest Department</name><woeid>55998384</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Cuvette Department</name><woeid>2344968</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Plateaux District</name><woeid>2344973</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Sangha</name><woeid>2344974</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Lekoumou</name><woeid>2344970</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Pool Department</name><woeid>2344975</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Likouala Department</name><woeid>2344971</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Niari Department</name><woeid>2344972</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Brazzaville</name><woeid>2344976</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Bouenza Department</name><woeid>2344967</woeid></place><place xmlns="http://where.yahooapis.com/v1/schema.rng"><name>Kouilou</name><woeid>2344969</woeid></place></results></query><!-- total: 19 -->\n<!-- engine7.yql.bf1.yahoo.com -->\n'
>>> rep = yql.rawQuery('select name, woeid from geo.states where place="Congo"', format='json')
>>> rep.json()
{u'query': {u'count': 11, u'lang': u'en-US', u'results': {u'place': [{u'woeid': u'55998384', u'name': u'Cuvette-Ouest Department'}, {u'woeid': u'2344968', u'name': u'Cuvette Department'}, {u'woeid': u'2344973', u'name': u'Plateaux District'}, {u'woeid': u'2344974', u'name': u'Sangha'}, {u'woeid': u'2344970', u'name': u'Lekoumou'}, {u'woeid': u'2344975', u'name': u'Pool Department'}, {u'woeid': u'2344971', u'name': u'Likouala Department'}, {u'woeid': u'2344972', u'name': u'Niari Department'}, {u'woeid': u'2344976', u'name': u'Brazzaville'}, {u'woeid': u'2344967', u'name': u'Bouenza Department'}, {u'woeid': u'2344969', u'name': u'Kouilou'}]}, u'created': u'2014-08-27T04:52:38Z'}}
>>>
```


Methods
-------

####use(yql_table_url,name=yql_table_name )
Changes the data provider

```python
>>> yql.use('http://www.josuebrunel.org//users.xml', name='myusers') 
```

####desc(tablename)
Returns table description
 
```python
>>> response = yql.desc('weather.forecast')
>>> response.json()
{u'query': {u'count': 1, u'lang': u'en-US', u'results': {u'table': {u'request': {u'select': [{u'key': [{u'required': u'true', u'type': u'xs:string', u'name': u'location'}, {u'type': u'xs:string', u'name': u'u'}]}, {u'key': [{u'required': u'true', u'type': u'xs:string', u'name': u'woeid'}, {u'type': u'xs:string', u'name': u'u'}]}]}, u'security': u'ANY', u'meta': {u'documentationURL': u'http://developer.yahoo.com/weather/', u'sampleQuery': u'select * from weather.forecast where woeid=2502265', u'description': u'Weather forecast table', u'author': u'Yahoo! Inc'}, u'hash': u'aae78b1462a6a8fbc748aec4cf292767', u'name': u'weather.forecast'}}, u'created': u'2014-08-16T19:31:51Z'}}
>>>
```

####rawQuery(query)

Allows you to directly type your query

```python
>>> response = yql.rawQuery("select * from geo.countries where place='North America'")
>>> # deal with the response
```

####select(table, fields, limit).where(filters, ...)

Select a table i.e *weather.forecast*.
If *table* not provided, it will use the default table. If there's no such thing as a default table, it will raise a *NoTableSelectedError*

***NB*** : A simple select doesn't return any data. Use ***GET*** instead.

```python
>>> response = yql.select('geo.countries', [name, code, woeid]).where(['name', '=', 'Canada'])
>>> response.json()
{u'query': {u'count': 1, u'lang': u'en-US', u'results': {u'place': {u'woeid': u'23424775', u'name': u'Canada'}}, u'created': u'2014-08-16T19:04:08Z'}}
>>> ...
>>> rep = yql.select('geo.countries', ['name', 'woeid'], 2).where(['place', '=', 'Africa'])
>>> rep.json()
{u'query': {u'count': 2, u'lang': u'en-US', u'results': {u'place': [{u'woeid': u'23424740', u'name': u'Algeria'}, {u'woeid': u'23424745', u'name': u'Angola'}]}, u'created': u'2014-08-17T10:52:49Z'}}
>>>
>>> rep = yql.select('geo.countries', ['name', 'woeid'], 2).where(['place', 'in', ('Africa', 'Europe')])
>>> rep.json()
{u'query': {u'count': 2, u'lang': u'en-US', u'results': {u'place': [{u'woeid': u'23424740', u'name': u'Algeria'}, {u'woeid': u'23424745', u'name': u'Angola'}]}, u'created': u'2014-08-17T11:22:49Z'}}
>>>
```

####get(table, fields, limit)
Same as ***SELECT***, but instead returns data.

**REMINDER** : Some tables require a **where clause**, therefore ***GET*** won't work on those tables, use *select(...).where(...)* instead .

```python
>>> rep = yql.get('geo.countries', ['name', 'woeid'], 1)
>>> rep.json()
{u'query': {u'count': 1, u'lang': u'en-US', u'results': {u'place': {u'woeid': u'23424966', u'name': u'Sao Tome and Principe'}}, u'created': u'2014-08-17T10:32:25Z'}}
>>>
```

####insert(table, (field1, field2, ..., fieldN),(value1, value2, ..., valueN))
Insert values into a table. Arguments 2 and 3 may be **tuples** or **list**.

```python
>>> response = yql.insert('yql.storage.admin',('value',),('http://josuebrunel.org',))
>>> response.json() # result prettyfied just for the example
{
    "query": {
        "count": 1,
        "created": "2015-05-14T13:25:56Z",
        "lang": "en-US",
        "results": {
            "inserted": {
                "execute": "store://KkkC5xDw4v32IcWWSQ4YRe",
                "select": "store://Zc5LHXcmYM7XBfSbo9tzFL",
                "update": "store://Rqb5fbQyDvrfHJiClWnZ6q"
            }
        }
    }
}
```

####update(table,[field1, ..., fieldN],[value1, ..., ...valueN]).where(filters, ...)
Update fields values. This method __is always followed by ***where()***__. Arguments 2 and 3 may be **tuples** or **list**.

```python
>>> response = yql.update('yql.storage',('value',),('https://josuebrunel.org',)).where(['name','=','store://Rqb5fbQyDvrfHJiClWnZ6q'])
>>> response.json() # result prettyfied just for the example
{
    "query": {
        "count": 1,
        "created": "2015-05-14T13:32:52Z",
        "lang": "en-US",
        "results": {
            "success": "Updated store://KkkC5xDw4v32IcWWSQ4YRe"
        }
    }
}
```

####delete(table).where(filters, ...)
Delete records
```python
>>> response = self.yql.delete('yql.storage').where(['name','=','store://Rqb5fbQyDvrfHJiClWnZ6q'])
>>> response.json() # result prettyfied just for the example
{
    "query": {
        "count": 1,
        "created": "2015-05-14T13:38:28Z",
        "lang": "en-US",
        "results": {
            "success": "store://Rqb5fbQyDvrfHJiClWnZ6q deleted"
        }
    }
}

```

Using OAuth to fetch protected resources
=========================================

```python
>>> from myql.contrib.auth import YOAuth
>>> oauth = YOAuth(None, None, from_file='credentials.json') # only consumer_key and consumer_secret are required.
>>> from myql import MYQL
>>> yql = MYQL(format='xml', oauth=oauth)
>>> response = yql.getGUID('josue_brunel') # Deal with the response
```

