# The Case/When Operation

* The case statement is a way to process a range of values into a set of discrete values.
* It can also be used to compare values from several columns in a row and generate a new column as a result. 
* If you're familiar with other programing languages, this is more or less SQL's if statement.
* Lets look at a couple examples:

```sql
select 
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname) as name,
  case
    when e.maritalstatus='M' then 'married'
	else 'single'
  end as martial_status
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid;
```

```sql
select 
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname) as name,
  case
    when e.maritalstatus='M' and e.gender='F' then 'married woman'
	when e.maritalstatus='M' then 'married man'
	when e.gender='F' then 'single woman'
	else 'single man'
  end as martial_status
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid;
```

* Notes: 
    * When values are considered in order... that's especially important to the behavior of the second of these which doesn't always specify the value of each relevant column, because it assumes the above `when`'s resulted in false.
    * You can combine boolean statements with and/or.
    * If you don't specify an else the value for unmatched cases will be `null`