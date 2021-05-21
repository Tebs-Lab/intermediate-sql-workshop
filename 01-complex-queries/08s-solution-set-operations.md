# Exercise: Set Operations

1. You've been charged with creating a query that powers a dropdown menu that can be used to select the name of every product individually as well as an option for "all products". Use a union statement to create such a query.

```sql
select 'All Products' as name
union all
(select name from production.product
order by name);
```

* Note: What happens if you remove the parenthesis?
    * Answer: it sorts the entire query AFTER the union happens. But we'd rather have "all" first followed by the alphabetical list of products.

2. Something bad happened and now you need to send an email to all of your customers as well as all of your employees, but not to any of your vendors. Create a query using `union` and `except` to select all the email addresses that belong to employees and customers as well as explicitly removing any email addresses that might belong to someone who is both a customer and a vendor or an employee and a vendor.

```sql
select 
  ea.emailaddress
from humanresources.employee e
join person.emailaddress ea on ea.businessentityid=e.businessentityid
union
	select 
	  ea.emailaddress
	from sales.customer c
	join person.emailaddress ea on ea.businessentityid=c.personid
union
	select 
	  ea.emailaddress
	from sales.customer c
	join person.businessentitycontact bec on bec.businessentityid=c.storeid
	join person.emailaddress ea on ea.businessentityid=bec.personid
except
	select 
		ea.emailaddress
		from purchasing.vendor v
		join person.businessentitycontact bec on bec.businessentityid=v.businessentityid
		join person.emailaddress ea on ea.businessentityid=bec.personid;
```

3. Create a query using `intersect` to identify all the lastnames that are shared by an employee and a customer that is an individual (rather than a store).

```sql
select 
  p.lastname
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid
intersect
	select 
	  p.lastname
	from sales.customer c
	join person.person p on c.personid=p.businessentityid;
```