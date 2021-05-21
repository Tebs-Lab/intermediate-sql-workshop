# Exercise: Views

Creating these views may require clever use of any number of the techniques we've learned about. There may also be more than one way to create a view that fits the needs described. 

1. Create a view for the HR department such that using `select * from YOUR_VIEW` a member of that department can know for each employee:
    * Their full name
    * Their email address
    * Their title
    * Which department they belong to currently
    * Their managers full name and email address

```sql
create view employee_manager as (
	with current_department as (
		select 
			businessentityid,
			departmentid
		from humanresources.employeedepartmenthistory edh1
		where edh1.startdate = (select max(edh2.startdate) 
								  from humanresources.employeedepartmenthistory edh2
								  where edh1.businessentityid=edh2.businessentityid
							   )
		order by businessentityid
	)

	select 
	  concat(emp_p.firstname, ' ', emp_p.middlename, ' ', emp_p.lastname) as employee_name,
	  emp_ea.emailaddress as employee_email,
	  emp.jobtitle as employee_title,
	  concat(manager_p.firstname, ' ', manager_p.middlename, ' ', manager_p.lastname) as manager_name,
	  manager_ea.emailaddress as manager_email,
	  d.name as department_name
	from humanresources.employee emp
	left join humanresources.employee manager on emp.organizationnode ~ concat('^', manager.organizationnode, '\d+/$')
	join current_department cd on cd.businessentityid=emp.businessentityid
	join humanresources.department d on d.departmentid=cd.departmentid
	join person.person emp_p on emp_p.businessentityid=emp.businessentityid
	join person.person manager_p on manager_p.businessentityid=manager.businessentityid
	join person.emailaddress emp_ea on emp_ea.businessentityid=emp.businessentityid
	join person.emailaddress manager_ea on manager_ea.businessentityid=manager.businessentityid
);

select * from employee_manager;
```

2. Create a view for the purchasing department such that using `select * from YOUR_VIEW` a member of that department can know about each vendor:
    * The company name
    * The point of contact's email address
    * How many products they sell to AdventureWorks
    * Whether or not they sell any bike pedals to AdventureWorks

```sql
create view vendors_contact_num_prods as (
	with products_per_vendor as (
		select
		  v.businessentityid,
		  count(1) as number_of_products
		from purchasing.vendor v
		join purchasing.productvendor pv on pv.businessentityid=v.businessentityid
		join production.product p on p.productid=pv.productid
		group by v.businessentityid
	),

	sells_bike_pedals as (
		select
		  v.businessentityid,
		  bool_or(p.name ~* 'pedal') as sells_pedals
		from purchasing.vendor v
		join purchasing.productvendor pv on pv.businessentityid=v.businessentityid
		join production.product p on p.productid=pv.productid
		group by v.businessentityid
	)

	select
	  v.name as vendor_name,
	  concat(p.firstname, ' ', p.middlename, ' ', p.lastname) as contact_name,
	  ea.emailaddress as contact_email,
	  sbp.sells_pedals,
	  pvp.number_of_products
	from purchasing.vendor v
	join person.businessentitycontact bc on bc.businessentityid=v.businessentityid
	join person.person p on bc.personid=p.businessentityid
	join person.emailaddress ea on ea.businessentityid=p.businessentityid
	join sells_bike_pedals sbp on sbp.businessentityid=v.businessentityid
	join products_per_vendor pvp on pvp.businessentityid=v.businessentityid
);

select * from vendors_contact_num_prods;
```


3. Choose your own view... In your teams decide on a view that could be useful for either the sales or production department. Try to make it challenging by: 
    * including information from multiple tables.
    * including information that must be computed with an aggregate
    * using a case statement and or regular expression