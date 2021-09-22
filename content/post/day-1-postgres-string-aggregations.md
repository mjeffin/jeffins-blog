---
title: "Day 1 - Postgres String Aggregations"
date: 2021-09-22T07:44:59+05:30
draft: false
tags: ["postgres","challenge"]
---
   
Aggregations in SQL are used to get useful insights from large set of data by doing some operations on it. Different scenarios of aggregations are,

1. Aggregate the entire result set into 1 single value. eg: max value of a column
2. Aggregate the entire result set into 1 single row. eg: max and min values of a column. Note that ALL the columns in this row have to be aggregates
3. Aggregate the entire result set into a _subset_ of rows, with 
   1. one row for each unique value for column specified by _group by_ clause. eg: max value of bill amount for each date
   2. one row for each unique combination of values for column**s** specified in _group by_ clause eg: max value of bill amount for each date for each store
   
When we talk about aggregations, usually mathematical or statistical operations like sum, avg,count etc comes to mind. 
Postgres also provides pretty good text [aggregation functions](https://www.postgresql.org/docs/13/functions-aggregate.html) also. Let's explore one use case

Suppose you are a smoke alarm sensor rental company. You have installed your equipments in different locations  
Assume for a moment that your database is not yet perfectly normalised or just that you are not storing Foreign keys in your time series 
table after reading some [guides](https://blog.timescale.com/blog/13-tips-to-improve-postgresql-insert-performance/) which says that FK's 
affect insert performance. So your schema for alarms table is like this,

|device_id|location|alarm_time|other fields.. |
| ---     | ---         |  ---     | ---          |

Then assume that you have a requirement to get the list of devices which generated the highest number of alarms in past 30 days. your query would be,

```sql
select device_id, count(*) as alarm_count
    from alarms
    where alarm_time > now() - interval '1 month'
    group by device_id order by alarm_count limit 10 
```

Now you know the devices which generated the highest number of alarms. Good. But the device id in itself doesn't tell you much. 
You give this to your safety officer, and he asks where these devices are located. After all, the location info is readily available in the table 
itself. But there is a catch.

>Once you do a group by, all the selected columns in that query has to be aggregated

You can't just specify a single column, you have to aggregate it.  So, how would you do that ? Well, you could use [string_agg](https://popsql.com/learn-sql/postgresql/how-to-use-stringagg-in-postgresql)
aggregation function. To eliminate duplicates, we can use the *distinct* keyword inside it. The final query is

```sql
select device_id, count(*) as alarm_count, string_agg(distinct location, '') 
    from alarms 
    where alarm_time > now() - interval '1 month'
    group by device_id order by alarm_count limit 10
```

Coming from the land of Django ORM, I have only started my pure SQL journey only about a year back. So, please let me know if there are 
better ways to do things that I mention in these posts. 

### Good Resouces

- https://dataschool.com/how-to-teach-people-sql/how-sql-aggregations-work/
- https://blog.timescale.com/blog/how-postgresql-aggregation-works-and-how-it-inspired-our-hyperfunctions-design-2/


That's it for Day 1. See you tomorrow!