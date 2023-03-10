# MongoDB

CRUD : [https://developer.mozilla.org/en-US/docs/Glossary/CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD)

JSON : [https://www.json.org/json-en.html](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)

Filtering : [https://www.mongodb.com/docs/compass/current/query/filter/](https://www.mongodb.com/docs/compass/current/query/filter/)

## MongoDB compass :

local tool to work with databases and collections with the UI and all CRUD operations are supported by it.

## Find :

JSON documents in mongodb are queried using json document based queries.

to query for a value of a field :

```
{field : value}
{name : "apple"}
{number : 12}
```

To have multiple conditions, just seperate them by comma :

```
{cond,cond2 ...}
{"word" : "apple" , "number" : 12}
```

## Comparision :

operators : 

```
{$op : {}}

{$op : []}

{field : {$op : {}}}

$gt,$lt,$gte,$lte
greater/less than equal_to

{"number" : {$lt : 13}}
```

## MongoShell or Mongosh :

```
show dbs //to show all databases
use db_name //to enter a particular db
db //refers to the currently selected db
show collectoins //to show the collections in a db
```

## Inserting documents :

```
db.collection_name.insertOne({..})
db.collection_name.insertMany([{..},{..},{..}])
```

## find :

```
db.collection_name.find({}) //limit 20
db.collection.find() // same as doing select * from collection
//find has two arguments, the first is the condition/filter the second is the list of fields to be included 
db.c1.find({}, {_id : 0, number : 1}) // 0 to exclude and 1 ot include.

db.collection.findOne() // same as doing select * from collection limit 1

```

## exists :

```
db.c1.find({"word" : {$exists : true}})
```

## Count, Sorting and limiting :

```
db.collection_name.find({predicate}).limit(no_of_documents) //includes no_of_documents documents
db.collection_name.find({predicate}).limit(no_of_documents) //starts from the no_of_documents + 1 th document
db.collection_name.find({predicate}).count() //select count(*) from collection_name where predicate
```

```
db.collection_name.find({predicate}).sort({field_name1:  1,field_name2:  1}) //1 for ascending -1 for descending.
//select * from collection_name where predicate order by field_name 
```

## Or operator :

```
db.collection_name.find({$or : [{prd1},{prd2}]})
```

## In keyword :

```
//to check existence in an array for a field.
db.collection_name.find({field_name : {$in : [val1,val2]}})
db.collection_name.find({field_name : {$nin : [val1,val2]}})
```

## and :

```
db.collection_name.find({$and: [{prd1},{prd2}]})
{$and : [{number : {$exists : true}}, {number : {$nin : [12,13]}} ]}
```

## Querying arrays :

1. to find if a value is in the array.
    
    ```
    db.collection_name.find({array_field : val}) //just checking the availability of val in the array_field
    ```
    
2. to match the array exactly
    
    ```
    db.collection_name.find({array_field : [val1,val2,val3 ...]}) //just checking the availability of val in the array_field
    ```
    
3. to see if all the passed values are in the array
    
    ```
    db.collection.find({arr_field : {$all :  [val1,val2 , ...]})
    ```
    
4. To check if the i’th element value is val :
    
    ```
    db.collection.find({"arr_field.i" : value})
    ```
    
    ### Deletion of documents :
    
    ```
    db.collection_name.deleteOne({pred})
    db.collection_name.deleteMany({pred})
    ```
    
    ## Updation of documents :
    
    ```
    db.collection_name.updateOne({pred}, {what_to_change})
    db.collection_name.updateMany({pred}, {what_to_change})
    ```
    
    ## Set operator :
    
    ```
    what_to_change -> $set : {field1 : value1 , field2 : value2}
    ```
    
    ## Increment operator :
    
    $inc
    
    ```
    what_to_change -> $inc: {field1 : increment_amount} //decrement can also be done using negative amounts
    ```
    
    ## Push-Pull :
    
    ```
    db.collection_name.updateOne({pred}, {  $pull : {array_field : value_to_remove}  })
    db.collection_name.updateOne({pred}, {  $push : {array_field : value_to_add}  })
    ```
    
    to add/remove several values : each keyword
    
    ```
    {$pull : {array_field : {$each : [val1,val2]}}
    ```
    
    ## Size operator :
    
    it is used to query size of array
    
    ```
    db.collection_name.find({arr_field : {$size : value}})
    ```
    
    ## Type operator :
    
    ```
    db.collection_name.find({field : {$type : value}}) //10 for null or just use "null"
    ```
    
    ## Not operator :
    
    ```
    db.col_name.find({field : {$not : {$eq : val}})
    db.col_name.find({field : {$not : {$gt : val}})
    ```
    
    ## Aggregation Pipeline :
    
    Input {stage} → Output1 {stage} → Output2
    
    Output2 {stage} → Output3
    
    Output2 {stage} → Output4
    
    ```
    db.collection_name.aggregate([{stage1},{stage2} , .....])
    ```
    
    ## Group :
    
    ```
    //group operator :
    {$group :
    {_id : "$groupField",
        aggregate_name : {aggregate_function} }}
    ```
    
    We are working with a collection with the schema g as group_id , v and v2 int values and a lis : array of int’s
    
    g,lis,v,v2
    A,"[10,20]",14,2
    A,"[10,20]",16,2
    B,"[20,30]",18,1
    B,"[20,30]",20,1
    C,[40],24,2
    C,[40],23,2
    C,[40],12,1
    C,[40],10,1
    
    ## Aggregate Functions
    
    ### Count
    
    ```
    db.c3.aggregate([{$group : {_id : "$g" , count_of_documents : {$count : {}}}}])
    // select count(*) as count_of_documents from c3 group by g
    ```
    
    ### Sum
    
    ```
    db.collection_name .aggregate([{$group : {_id : "$field_to_group" , count_of_documents : {$sum : "$field_to_sum"}}}])
    // select sum(field_to_sum) as alias from collection_name group by field_to_group
    ```
    
    ### Push
    
    ```
    db.collection_name .aggregate([{$group : {_id : "$field_to_group" , array_of_group : {$push: "$field_to_push"}}}])
    
    db.c3.aggregate([{$group : {_id : "$g" , array_of_group : {$push: "$v"}}}])
    ```
    
    ## Match :
    
    ```
    db.collection_name.aggregate([{$match : {predicate}}]) //match is like a where clause
    select * from collection_name where predicate
    ```
    
    **********************************Match and Group :**********************************
    
    ```
    db.c3.aggregate([{$match : {v : {$gt : 15}}},{$group : {_id : "$g" , sum_of_v : {$sum :"$v" }}}])
    
    //select sum(v) from c3 where v > 15 group by g
    ```
    
    ```
    agg[{match1},{group},{match2}]
    
    db.c3.aggregate([{$match : {v : {$gt : 15}}},{$group : {_id : "$g" , sum_of_v : {$sum :"$v" }}},
    {$match : {sum_of_v : {$gte : 20}}}])
    
    //select sum(v) as sum_of_v from c3 where v > 15 group by g having sum(v) >= 20
    ```
    
    ### Sort :
    
    ```
    {$sort : {field_to_sort_by : 1}}
    
    ```
    
    ### Limit and Skip are also stages :
    
    ```
    {$limit : n1}
    {$skip : n2}
    
    db.c3.aggregate([{ $match: { v: { $gt: 15 } } }, { $group: { _id: "$g", sum_of_v: { $sum: "$v" } } },{$sort : {sum_of_v : -1}},{$skip : 1}, {$limit : 1}])
    ```
    
    ## Project :
    
    1. To include fields :
    
    ```
    {$project : {field_name_1 : 1 , field_name_2: 0}} //1 to include 0 to exclude
    
    db.c3.aggregate([{$project : {_id: 0}}])
    ```
    
    1. To alias fields :
    
    ```
    {$project : {field_name_1_alias : "$field_name_1" , ****field_name_2_alias : "$field_name_2"}} //1 to include 0 to exclude
    ```
    
    1. Expression evaluation : 
        
        ```
        {$project : {alias : {expr}}} //1 to include 0 to exclude
        
        db.c3.aggregate([{$project : {_id : 0 , v : 1, v2 : 1,v1_sum_v2 : {$multiply : ["$v","$v2"]}}}])
        ```
        
    
    ## Unwind :
    
    for one document → produce multiple documents one for each value of the list that has been unwound.
    
    {user_id : 1,ph_no : 93993837754}
    
    {user_id : 2,ph_no : [937467634874,94777356289]}
    
    →
    
    {user_id : 1,ph_no : 93993837754}
    
    {user_id : 2,ph_no :94777356289}
    
    {user_id : 2,ph_no : 937467634874}
    
    for each user : fetch user.phoneNumber  → p
    
    message(p : Long, “this is  a notification”)
    
    ```
    {$unwind : "$array_to_unwind"}
    ```