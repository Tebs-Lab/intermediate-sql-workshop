# Group By

* The purpose of group by is to compute aggregates over a set of rows that all share a value in one or more column. 

* Some examples of the available aggregates are:
    * min,
    * max,
    * count,
    * sum,
    * average
    * More: https://www.postgresql.org/docs/13/functions-aggregate.html

* Demonstrate with this query:

```sql
select 
  customerid,
  count(1) as number_of_purchases,
  sum(totaldue) as total_spent,
  avg(totaldue) as avg_purchase_amount,
  min(totaldue) as smallest_purchase,
  max(totaldue) as largest_purchase
from 
  sales.salesorderheader
group by 
  customerid
order by 
  number_of_purchases desc,
  total_spent desc,
  avg_purchase_amount desc;
```

* Notes:
    * `count(1)` count increments for any non-null value, but we're aggregating a non-null field (every entry in salesorderheader represents an actual sale, so we want to increment for EVERY record). So count(1) saves a small amount of computation. count(totaldue) would return the same results (and you can prove this by changing the query!)
    * order by the aliases doesn't work in all versions of SQL (or even all versions of Postgres). We must repeat the aggregates in older versions.

* We can also:
    * limit records prior to the grouping, using `where`
    * limit records after the grouping, using `having`
    
* So, what's different about how this query will be processed?

```sql
select 
  customerid,
  count(1) as number_of_purchases,
  sum(totaldue) as total_spent,
  avg(totaldue) as avg_purchase_amount,
  min(totaldue) as smallest_purchase,
  max(totaldue) as largest_purchase
from 
  sales.salesorderheader
where
  totaldue > 100
group by 
  customerid
having 
  count(1) > 5 -- note: cannot use aliases in having or where
order by 
  number_of_purchases desc,
  total_spent desc,
  avg_purchase_amount desc;
```

* Notes:
    * The `where` prevents us from considering any purchases of less than $100 BEFORE we compute the aggregates, which is why the largest "number of purchases" is smaller.
    * The `having` filters rows after the aggregates, meaning our report doesn't include any records for customers who have made fewer than 5 purchases. 

* Group by plays nice with join. All the joins happen before the `group by` is processed.