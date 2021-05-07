# Regular Expressions

* Regular expressions are common in many programing languages, and are often helpful in searching for patterns in text.
* Most SQL databases support regex.
* Regex is powerful/complex and we can't possibly cover all of its features today...
* Postgres provides the similar but simplified `like/ilike` constructs which can be used as a faster replacement for some simple regular expressions.
* Lets learn by example:

* `like`/`ilike`
    * Offers us 2 special match properties.
    * `%` is any string of characters of any length
    * `_` is any single character
    * `ilike` is case insensitive

```sql
select 
  firstname 
from person.person
where 
  -- This is equivalent to firstname='Ann'
  firstname like 'Ann';

select 
  firstname 
from person.person
where 
  -- This is any name that starts with Ann
  firstname like 'Ann%';

select 
  firstname 
from person.person
where 
  -- This is any name that starts with Ann
  -- and is exactly 4 letters long, but the 4th 
  -- letter can be anything
  firstname like 'Ann_';

select 
  firstname 
from person.person
where 
  -- This is any name that ends with z
  firstname like '%z';

select 
  firstname 
from person.person
where 
  -- This is any name that contains 'an' somewhere
  -- Note, case sensitive!
  firstname like '%an%';
```

* `like/ilike` are super useful for simple patterns, and faster than regex.
* `~` is the regex operator and `~*` is case insensitive regex.
* Regex offers some very fancy features that can be very helpful...
    * `.` is regex's version of `_` — it matches any one character.
    * `*` and `+` are quantitative operations that modify the previous character:
        * `.*` means "0 or more of any character"
        * `.+` means "one or more of any character"
        * `a+` means "one or more a's in a row"
    * More specific quantitive operations use `{}`
        * `a{1,3}` means between 1 and 3 a's in a row.

```sql
select 
  firstname 
from person.person
where 
  -- an anywhere in regex:
  firstname ~ '.*an.*';

select 
  firstname 
from person.person
where 
  -- two consecutive t's
  firstname ~ 't{2}';

select 
  firstname 
from person.person
where 
  -- What about this one?
  firstname ~ 'a.t';
```


* `^` matches the start of a string and `$` matches the end of a string.

```sql
select 
  firstname 
from person.person
where 
  -- Starts with A
  firstname ~ '^A';

select 
  firstname 
from person.person
where 
  -- ends with z
  firstname ~ 'z$';
```

* Now some fancy stuff!
* Character classes are grouped in `[]`
    * A character class is a set of characters to match.
    * The `-` is used to specify characters "between" each other, which has to do with the `ASCII` and `Unicode` formats and their lookup tables.
    * But simply, `[a-z]` means all the lowercase letters from a to z. `[a-zA-Z] means all lower and uppercase letters.
    * `[aeiou]` means any of the vowels. 
* Character classes can be combined with the numerical operators!

```sql
select 
  firstname 
from person.person
where 
  -- two or more consecutive vowels
  firstname ~ '[aeiou]{2,}';
```

* `()` can be used to create groups, which allow for the use of the or operator `|`
    * There are other uses of the group operator, but we don't have time to get into them...

```sql
select 
  firstname 
from person.person
where 
  -- contains at least one of the specific strings ae or ea
  firstname ~ '(ae|ea)';
```

* There are a lot of special values baked in for common needs, such as matching any punctuation, any number, any whitespace characters, and much more
    * Check out these tables: [https://www.postgresql.org/docs/13/functions-matching.html#POSIX-CHARACTER-ENTRY-ESCAPES-TABLE](https://www.postgresql.org/docs/13/functions-matching.html#POSIX-CHARACTER-ENTRY-ESCAPES-TABLE)
