# Partition By / Window Functions

* Window functions are an alternative way to use aggregates. 
* They are different in two ways from the standard `group by`:
    * They don't collapse rows into the grouping, instead we read every row AND compute the aggregate over some specified groups.
    * We can use them to include multiple different groupings in a single query.

* Consider this queries with the class:

```sql
select 
  *,
  avg(taxrate) over (partition by taxtype) as avg_by_type,
  avg(taxrate) over (partition by stateprovinceid) as avg_by_state_province
from sales.salestaxrate
order by 
  taxtype,
  stateprovinceid;
```

* Just like with group by, we can apply these to queries with joins and other complex features. 
* The "partition by" rules are applied at the same time that a group by would be applied.
* We can even use window functions on statements where Group By has been used, and in this case the partition by will be applied to the **result of the grouping**
    * Be careful when doing this, especially with `avg` since taking the average of an average can result in errors.