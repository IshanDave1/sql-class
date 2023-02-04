# SQL

## Select :

tables : t

cols : col1,col2…. (or a,b,c……)

```sql
SELECT * FROM t
SELECT col1,col2... FROM t
SELECT expression FROM t
select 1 from t #returns just value 1 repeated for all rows
select col1*2 from t #returns twice the first column value
```

## ALIAS

```sql
SELECT expression as alias from table_name #the expression is now desplayed with the alias column name
```

## Where :

Lets you filter data depending on condition

```sql
SELECT col1,col2.... 
FROM t
WHERE condition1 and/or/not condition2 and/or/not condition3
```

```sql
CREATE TABLE t (
  `col1` INTEGER,
  `col2` VARCHAR(1)
);

INSERT INTO t
  (`col1`, `col2`)
VALUES
  ('1', NULL),
  ('2', 'b'),
  ('3', 'c'),
  ('4', 'd'),
  ('5', 'e');
```

```sql
#get col1 > 2
SELECT * FROM t
WHERE col1 > 2

#filter on string based column -> put it inside ''
SELECT * FROM t
WHERE col2 = 'a'
```

| Name | Description |
| --- | --- |
| > | Greater than operator |
| => | Greater than or equal operator |
| <= | Less than operator |
| <>, != | Not equal operator |
| <= | Less than or equal operator |
| <=> | NULL-safe equal to operator |
| = | Equal operator |
| BETWEEN … AND … | Whether a value is within a range of values |
| COALESCE() | Return the first non-NULL argument |
| GREATEST() | Return the largest argument |
| IN() | Whether a value is within a set of values |
| INTERVAL() | Return the index of the argument that is less than the first argument |
| IS | Test a value against a boolean |
| IS NOT | Test a value against a boolean |
| IS NOT NULL | NOT NULL value test |
| IS NULL | NULL value test |
| ISNULL() | Test whether the argument is NULL |
| LEAST() | Return the smallest argument |
| LIKE | Simple pattern matching |
| NOT BETWEEN ... AND … | Whether a value is not within a range of values |
| NOT IN() | Whether a value is not within a set of values |
| NOT LIKE | Negation of simple pattern matching |
| STRCMP() | Compare two strings |

Expresssions that return boolean values, can return 3 types of values :

1 → true

0 → false

null

Querying for NULL/NON-NULL columns can be done by ‘=’, you need to use the `is NULL/is NOT NULL` keyword

```sql
SELECT * FROM t
WHERE col2 is NULL
```

**IN :** lets you choose from a list of values.

```sql
SELECT * FROM t
where col1 in (1,3,5)
```

********LIKE :******** lets you match patterns using wildcards, 

% → anything goes

_ → single char

| movie_name like ‘The%’ | gives all movies that start with The |
| --- | --- |
| movie_name like ‘%and%’ | gives all movies that have and somewhere in the middle |
| column_name like ‘a__’ | match all columns of length 3 starting with a |

## Limit :

lets you get a certain number of rows

## Offset

lets you do the limit operation starting after the offset number

```sql
select * from countries
where Country LIKE 'A%'
limit 5
offset 1
```

## Distinct :

selects distinct combination of values from the rows only for the selected ones

```sql
CREATE TABLE t (
  `a` INTEGER,
  `b` VARCHAR(1),
  `c` VARCHAR(1)
);

INSERT INTO t
  (`a`, `b`, `c`)
VALUES
  ('1', 'a', 'A'),
  ('1', 'a', 'A'),
  ('1', 'a', 'B'),
  ('1', 'b', 'B'),
  ('1', 'b', 'C'),
  ('2', 'a', 'A'),
  ('2', 'a', 'A'),
  ('2', 'a', 'B'),
  ('2', 'b', 'B'),
  ('2', 'b', 'C');
```

```sql
select distinct a , b from t
#returns
#1 a
#1 b
#2 a
#2 b
#NOTE distinct is being applied to both a and b not just to a
```

## Count

to count total number of rows :

```sql
CREATE TABLE t (
  `col1` INTEGER,
  `col2` VARCHAR(1),
  `col3` VARCHAR(1)
);

INSERT INTO t
  (`col1`, `col2`, `col3`)
VALUES
  (NULL, 'a', 'A'),
  ('1', 'a', 'A'),
  ('1', 'a', 'B'),
  ('1', NULL, 'B'),
  ('1', NULL, 'C'),
  ('2', NULL, 'A'),
  ('2', 'a', 'A'),
  ('2', 'a', 'B'),
  ('2', 'b', 'B'),
  (NULL, 'b', 'C');
```

```sql
SELECT COUNT(*) FROM table_name
```

to count total non null values for a particular column :

```sql
SELECT count(col_name) FROM table_name
```

to count total non null distinct values for a particular column :

```sql
SELECT count(distinct col_name) FROM table_name
```

```sql
select count(col1) as col1_count, count(col2) as col2_count
from t
#RETURNS 8,7
```

## GROUP BY

creates groups for the columns put inside the `group by` clause, all the columns in the same group get coalesced down into one. The only ppt of a group that can now be selected is the group columns and aggregates(count,sum,max,min)

```sql
select cols 
from table
group by g_cols

#select can only have columns that are present in group by and aggregates.
```

## ORDER BY

lets you output based on the value of a particular column(s) 

if there are multiple cols inside order the 2,3,4… column will be tie-breakers

```sql
select cols 
from table
group by g_cols
order by o_cols
```
