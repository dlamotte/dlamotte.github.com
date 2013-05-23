---
layout: post
title: "Custom SQL with Django ORM"
tags: django orm
---

When faced with the need to do more complex things with the ORM in Django, it
can be pretty frustrating.  I happened upon a pretty interesting solution I'd
like to share.

I needed to do some simple `GROUP BY`, but this trick can apply to all kinds
of complicated SQL.  The beauty of the ORM is that you can effectively pass
around a SQL query and extend it.  It encapsulates building a SQL String and
saving associated params with that query.  So, now, you want to do something
more complicated, you need to use the "Escape Hatch" and use custom SQL, but
you still need to begin with a `QuerySet` as a base.  Here's how:

```
from django.db import connections, router
from myproject.models import MyModel

qs = MyModel.objects.filter(...) # feel free to use the QuerySet API however

def do_custom_sql(qs):
    conn = connections[router.db_for_read(qs.model)]
    cursor = conn.cursor()

    # build the SQL and extract the params
    sql, params = qs.query.get_compiler(router.db_for_read(qs.model)).as_sql()

    # extract the WHERE clause
    where = sql[sql.index('WHERE') + 5:]

    cursor.execute('''
        SELECT
            column1
            , count(column2) as hits
        FROM
            %(table)s
        WHERE
            %(where)s
        GROUP BY
            column1
        HAVING
            hits > 100
    ''' % {
        'table': conn.ops.quote_name(qs.model._meta.db_table),
        'where': where,
    }, params)

    for column1, hits in cursor.fetchall():
        ...
```

Now you can use the power of the ORM with the power of custom SQL.  Of course
there are caveats, but at least this is in your toolbox now and you'll probably
be able to figure it out from there.
