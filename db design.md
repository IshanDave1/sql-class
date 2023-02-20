# DB design

Designing a a db :

decide the tables you’ll have and the columns that each table will have

## Keys :

The problems that arise when working with db’s comes from :

1. NULL’s
2. Duplicates

**Primary key : unique identifier for entities in a table.**

1. No duplicates allowed
2. Non Null

```sql
alter table tb add primary key(key_col);
```

school db

student table :

studentid → pk

**Composite Key :** 

student tb

**roll_no , class , section**, name

roll_no → 1- num of students in a class

******Candidate Keys :******

state table :

**state_name, state_abb**, capital

**Alternate Keys :**

other keys from the potential PK’s and unqiues are AK’s

```sql
alter table tb_name add unique(unique_col);
```

**Foreign Keys :**

if tb1 has a PK as a column fk_col and tb2 and tb1 are related many to 1 then , col_1 is a FK in tb2.

```sql
alter table tb2 add foreign key(fk_col) references tb1(pk_col)

#customer -> id,name ...............
#order-> id,c_id ..........

alter table orders add foreign key(c_id) references tb1(id)
```

## Types of relations between tables :

1. one to one
2. one to many
3. many to many

customer :

id,name

order

id,time,cust_id

library management

book

id,name

author

id,name,bid

student

id,name,course

1,Shiva,1

1,Shiva,2

1,Shiva,3

1,Shiva,4

course

id,name

## Normalisation :

Anomalies :

1. insertion anomally
2. deletion anomally
3. updation anomally

## 1NF :

1. must not have any row order implicit, if it is so make it into a column explicitly
    
    movie
    
    id,name → id,name,year
    
2. must not have mixed data type (movie year a exact and between 2002 and 2004)
3. must have a PK
4. must not have repeating group of info in a single row

customer,

id,name, {phone no info} 

1. phone no list
2. ph1,ph2,ph3,ph4
3. create another table :
    
    phoneNumbers : c_id,ph_no
    

## 2NF :

1. each non key attribute must depend on the entire PK

student :

**roll_no,class,section**, shift_timings

shifts table :

class ,shift_timings

## 3NF

forbids dependency of non key attribute on another non key attribute :  thus non key attribute must only depend on key

student 

id,name,percentage,grade

percentage,grade table