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

**IS NULL :** Querying for NULL/NON-NULL columns can be done by ‘=’, you need to use the `is NULL/is NOT NULL` keyword

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

% → any number of characters.

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

## Aggregate :

```sql
SELECT agg_fn(col)
FROM table
```

The functions `SUM`, `COUNT`, `MAX`,`MIN` and `AVG` are "aggregates", each may be applied to a numeric attribute resulting in a single row being returned by the query

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

select *,
rank() over(partition by class,section order by marks),
max(marks) over (partition by class,section)
from stu
order by class,section

## Having

To filter on top of aggr columns, 

```sql
select cols 
from table_name
where normal_condition
group by g_cols
having aggregate_condition #this applies to aggr columns
```

```sql
CREATE TABLE stu (
  `student` VARCHAR(10),
  `gender` VARCHAR(1),
  `class` INTEGER,
  `section` VARCHAR(1),
  `marks` INTEGER
);

INSERT INTO stu
  (`student`, `gender`, `class`, `section`, `marks`)
VALUES
  ('Buckie', 'F', '10', 'C', '49'),
  ('Fonsie', 'F', '2', 'A', '48'),
  ('Atlanta', 'F', '7', 'C', '87'),
  ('Christa', 'F', '10', 'B', '85'),
  ('Bettina', 'M', '5', 'D', '80'),
  ('Mile', 'F', '3', 'B', '80'),
  ('Natty', 'F', '8', 'A', '15'),
  ('Marcela', 'F', '7', 'C', '92'),
  ('Nicola', 'M', '8', 'B', '21'),
  ('Madalena', 'F', '3', 'C', '56'),
  ('Hinda', 'F', '10', 'A', '15'),
  ('Aviva', 'M', '9', 'B', '100'),
  ('Adolpho', 'M', '9', 'A', '94'),
  ('Ann', 'F', '7', 'A', '93'),
  ('Minni', 'F', '5', 'C', '91'),
  ('Salomon', 'F', '12', 'B', '43'),
  ('Roslyn', 'F', '7', 'A', '88'),
  ('Krisha', 'F', '1', 'B', '44'),
  ('Murry', 'F', '11', 'D', '17'),
  ('Howey', 'M', '10', 'D', '54'),
  ('Noland', 'M', '7', 'D', '88'),
  ('Celestyn', 'F', '10', 'C', '80'),
  ('Christal', 'F', '6', 'D', '2'),
  ('Tilly', 'F', '12', 'D', '7'),
  ('Leon', 'F', '8', 'C', '64'),
  ('Sayer', 'F', '7', 'A', '76'),
  ('Brock', 'F', '9', 'C', '49'),
  ('Daryl', 'F', '5', 'C', '36'),
  ('Niall', 'F', '5', 'D', '100'),
  ('Frank', 'M', '5', 'B', '47'),
  ('Wit', 'F', '11', 'A', '30'),
  ('Mirilla', 'M', '8', 'B', '99'),
  ('Erda', 'M', '6', 'A', '11'),
  ('Vassili', 'M', '10', 'D', '76'),
  ('Corenda', 'M', '8', 'A', '65'),
  ('Nancey', 'M', '4', 'B', '61'),
  ('Beniamino', 'M', '9', 'D', '40'),
  ('Terri', 'F', '6', 'C', '76'),
  ('Ardisj', 'M', '10', 'A', '88'),
  ('Marj', 'F', '6', 'D', '60'),
  ('Kenyon', 'M', '10', 'D', '76'),
  ('Sunshine', 'M', '6', 'B', '8'),
  ('Cicily', 'M', '1', 'A', '16'),
  ('Karim', 'M', '11', 'B', '46'),
  ('Wayne', 'F', '11', 'B', '85'),
  ('Brittani', 'M', '3', 'B', '83'),
  ('Madelina', 'F', '1', 'D', '39'),
  ('Agosto', 'M', '1', 'A', '100'),
  ('Trip', 'F', '9', 'B', '58'),
  ('Phaidra', 'F', '4', 'C', '73'),
  ('Emmeline', 'M', '8', 'B', '40'),
  ('Vick', 'F', '1', 'C', '8'),
  ('Kerstin', 'F', '7', 'D', '37'),
  ('Teodor', 'M', '4', 'C', '7'),
  ('Daron', 'F', '11', 'B', '62'),
  ('Kerby', 'M', '12', 'B', '99'),
  ('Jehu', 'M', '5', 'B', '44'),
  ('Lindi', 'F', '10', 'A', '76'),
  ('Janaya', 'F', '10', 'C', '50'),
  ('Raffarty', 'M', '8', 'D', '51'),
  ('Dieter', 'M', '6', 'C', '50'),
  ('Alfons', 'F', '7', 'B', '88'),
  ('Tedi', 'M', '3', 'A', '6'),
  ('Cynthy', 'M', '1', 'B', '77'),
  ('John', 'M', '3', 'A', '13'),
  ('Delilah', 'M', '10', 'C', '78'),
  ('Andonis', 'M', '10', 'C', '28'),
  ('Maudie', 'F', '4', 'C', '82'),
  ('Staffard', 'F', '5', 'A', '77'),
  ('Edmund', 'M', '1', 'A', '75'),
  ('Wallis', 'F', '12', 'C', '3'),
  ('Sheridan', 'F', '2', 'B', '8'),
  ('Prent', 'F', '5', 'A', '80'),
  ('Guinna', 'M', '4', 'C', '48'),
  ('Paolina', 'M', '5', 'C', '54'),
  ('Rebbecca', 'M', '8', 'D', '93'),
  ('Marjorie', 'M', '9', 'C', '43'),
  ('Fremont', 'M', '4', 'A', '10'),
  ('Bard', 'M', '10', 'C', '16'),
  ('Harlen', 'M', '4', 'C', '28'),
  ('Brietta', 'F', '8', 'A', '48'),
  ('Uta', 'F', '1', 'D', '18'),
  ('Gal', 'F', '4', 'C', '32'),
  ('Krysta', 'F', '11', 'B', '43'),
  ('Corri', 'F', '9', 'C', '59'),
  ('Annaliese', 'F', '5', 'D', '30'),
  ('Erskine', 'M', '9', 'C', '56'),
  ('Arther', 'F', '10', 'C', '25'),
  ('Tiffanie', 'M', '5', 'D', '90'),
  ('Vita', 'M', '11', 'D', '45'),
  ('Belle', 'M', '6', 'B', '17'),
  ('Christabel', 'M', '1', 'B', '12'),
  ('Cayla', 'F', '10', 'B', '91'),
  ('Elia', 'F', '7', 'D', '4'),
  ('Elfrieda', 'M', '8', 'B', '89'),
  ('Alexio', 'M', '3', 'D', '1'),
  ('Bettina', 'M', '2', 'D', '67'),
  ('Enoch', 'M', '6', 'B', '29'),
  ('Arleta', 'M', '3', 'C', '39'),
  ('Artie', 'F', '11', 'C', '38');
```

```sql
select class,section,count(*) as total_in_class
from stu
group by class, section
having total_in_class > 2
order by class, section
```

## IF

if takes three args, first is condition and the value is second arg if the condition is true else the third arg

```sql
select col, IF(cond,cond_true_expr,cond_false_expr)
from tb
```

## Case

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

```sql
select 
  student,
  marks,
  CASE
    WHEN marks<20 THEN 'E'
    WHEN marks<40 THEN 'D'
    WHEN marks<60 THEN 'C'
    WHEN marks<80 THEN 'B'
    ELSE 'A'
  END as grade
from stu
```

## Join

1. output table gets all columns from the first and the second table
2. works as a filter on a cross

![Untitled](SQL%207af36ad707984736b98c5d2f464267dc/Untitled.png)

JOIN  →

1. output table with schema from both
2. Cross
3. apply the join condition as a filter
4. see if it is left.right.inner

```sql
SELECT cols from table1 [type_of_join] table2 ON condition
```

```sql
#t1 -> a t2->a,b
SELECT t1.a , b from t1 left join t2 on t1.a = t2.a 
```
