Sandals
=======

.. image:: https://raw.githubusercontent.com/jbochi/sandals/master/logo.jpg
  :scale: 50 %

.. image:: https://travis-ci.org/jbochi/sandals.svg?branch=master
  :alt: Travis CI build status
  :target: https://travis-ci.org/jbochi/sandals

.. image:: https://coveralls.io/repos/jbochi/sandals/badge.svg?branch=master
  :alt: Coveralls badge
  :target: https://coveralls.io/r/jbochi/sandals?branch=master


Use SQL to transform pandas_ dataframes.

Unlike pandasql_, sandals does not use sqlite. It manipulates the dataframes directly, avoiding the
overhead of moving data to and from sqlite.

This is a work in progress. The goal is to support all examples documented on pandas' `comparison with sql`__.

.. _pandas: http://pandas.pydata.org/
.. _pandasql: https://github.com/yhat/pandasql
.. __: http://pandas.pydata.org/pandas-docs/dev/comparison_with_sql.html

Usage
-----

Some examples:

.. code-block::

  >>> import pandas as pd
  >>> import sandals
  >>> tips = pd.read_csv("tests/data/tips.csv")
  >>> sandals.sql("SELECT total_bill, sex FROM tips LIMIT 5", locals())
     total_bill     sex
  0       16.99  Female
  1       10.34    Male
  2       21.01    Male
  3       23.68    Male
  4       24.59  Female
  >>> sandals.sql("SELECT * FROM tips WHERE time = 'Dinner' AND tip > 5.00 LIMIT 3", locals())
      total_bill   tip   sex smoker  day    time  size
  23       39.42  7.58  Male     No  Sat  Dinner     4
  44       30.40  5.60  Male     No  Sun  Dinner     4
  47       32.40  6.00  Male     No  Sun  Dinner     4
  >>> sandals.sql("SELECT * FROM tips ORDER BY tips.size DESC, total_bill LIMIT 3", locals())
       total_bill  tip     sex smoker   day   time  size
  143       27.05  5.0  Female     No  Thur  Lunch     6
  125       29.80  4.2  Female     No  Thur  Lunch     6
  141       34.30  6.7    Male     No  Thur  Lunch     6
  >>> sandals.sql("SELECT tips.day, AVG(tip), COUNT(*) FROM tips GROUP BY tips.day;", locals())
             tip  day
  day
  Fri   2.734737   19
  Sat   2.993103   87
  Sun   3.255132   76
  Thur  2.771452   62
  >>> sandals.sql("SELECT smoker, tips.day, COUNT(*), AVG(tip) FROM tips GROUP BY smoker, tips.day;", locals())
                    tip  smoker
  smoker day
  No     Fri   2.812500       4
         Sat   3.102889      45
         Sun   3.167895      57
         Thur  2.673778      45
  Yes    Fri   2.714000      15
         Sat   2.875476      42
         Sun   3.516842      19
         Thur  3.030000      17
