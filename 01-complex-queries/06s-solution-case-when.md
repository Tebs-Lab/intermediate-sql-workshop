# Exercise: Case/When

1. Use a case statement to select the name of each product in the `product` table as well as a column named "customer_segment" that contains:
    1. "Luxury" if the list price is more than $1500
    2. "HENRY" if the list price is between $1500 and $500
    3. "Standard" if the list price is less than $500

```sql
select 
  name,
  listprice,
  case
    when listprice > 1500 then 'Luxury'
	when listprice > 500  then 'HENRY'
	else 'Standard'
  end as product_type
from production.product
order by listprice;
```

2. AdventureWorks is initiating a mandatory vacation policy for employees who have accumulated too many vacation hours. Write a query that uses case/when and does the following:
    1. Ignores inactive employees.
    2. Includes the employee's name, role, number of accumulated vacation hours and a column which reads:
        * "Mandatory vacation" if the vacation hours are > 40
        * "gentle reminder" if the vacation hours are between 39 and 30
        * "No action" otherwise.

```sql
select 
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname) as name,
  case
    when vacationhours > 40 then 'Mandatory vacation'
	when vacationhours > 30 then 'Gentle reminder'
	else 'no action'
  end as vacation_action
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid
where currentflag=true
order by vacation_action;
```

3. Use an aggregate and a case statement to select all `purchasing.productvendors` and a column that indicates for each entry if that productvendor has a higher `averageleadtime` than the global average across all products. That column should read "More Lead Time" if so, and "Standard Lead Time" if not.

```sql
select 
  p.name,
  v.name,
  pv.averageleadtime,
  case 
    when 
	  averageleadtime > (select avg(averageleadtime) from purchasing.productvendor)
	  then 'More Lead Time'
	else 'Standard Lead Time'
  end
from purchasing.productvendor pv
join purchasing.vendor v on v.businessentityid=pv.businessentityid
join production.product p on p.productid=pv.productid
order by averageleadtime;
```