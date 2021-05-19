# Regular Expressions Exercise

1. Write a query that identifies all the person.person whose name contains a capital or lowercase z anywhere in their name.

```sql
-- With ilike
select 
  concat(firstname, middlename, lastname) 
from person.person 
where
  concat(firstname, middlename, lastname) ilike '%z%';

-- With regex
select 
  concat(firstname, middlename, lastname) 
from person.person 
where
  concat(firstname, middlename, lastname) ~ '.*[Zz].*';

-- case insensitive regex
select 
  concat(firstname, middlename, lastname) 
from person.person 
where
  concat(firstname, middlename, lastname) ~* '.*z.*';
```

2. Write a query to identify all the products whose names contain the word "washer" or "nut".

```sql
select 
  name 
from production.product
where
  name ~* '.*(washer|nut).*';
```

3. Write a query to identify all the products that are some kind of "HL" "Frame" and either "Black" or "Yellow"
    * Hint: select all the names in the product table to identify a pattern that will help you write this regular expression

```sql
select 
  name 
from production.product
where
  name ~* 'HL.*Frame.*(Black|Yellow)';
```

4. Write a query to select all the descriptions from the `productdescription` table that exclusively use letters from the English alphabet (lowercase or uppercase), whitespace characters, and punctuation.

```sql
select 
  description
from production.productdescription
where
  description ~ '^[a-zA-Z\s[:punct:]]+$';
```

5. Write a query to select all the descriptions from the `productdescription` table that contain any numbers.

```sql
select 
  description
from production.productdescription
where
  description ~ '\d';
```
