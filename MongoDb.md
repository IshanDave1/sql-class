# MongoDB

CRUD : [https://developer.mozilla.org/en-US/docs/Glossary/CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD)

JSON : [https://www.json.org/json-en.html](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)

Filtering : [https://www.mongodb.com/docs/compass/current/query/filter/](https://www.mongodb.com/docs/compass/current/query/filter/)

## MongoDB compass :

local tool to work with databases and collections with the UI and all CRUD operations are supported by it.

## Find :

JSON documents in mongodb are queried using json document based queries.

to query for a value of a field :

```json
{field : value}
{name : "apple"}
{number : 12}
```

To have multiple conditions, just seperate them by comma :

```json
{cond,cond2 ...}
{"word" : "apple" , "number" : 12}
```

## Comparision :

operators : 

```json
{$op : {}}

{$op : []}

{field : {$op : {}}}

$gt,$lt,$gte,$lte
greater/less than equal_to

{"number" : {$lt : 13}}
```

## MongoShell or Mongosh :

```json
show dbs //to show all databases
use db_name //to enter a particular db
db //refers to the currently selected db
show collectoins //to show the collections in a db
```

## Inserting documents :

```json
db.collection_name.insertOne({..})
db.collection_name.insertMany([{..},{..},{..}])
```

## find :

```json
db.collection_name.find({}) //limit 20
db.collection.find() // same as doing select * from collection
//find has two arguments, the first is the condition/filter the second is the list of fields to be included 
db.c1.find({}, {_id : 0, number : 1}) // 0 to exclude and 1 ot include.

db.collection.findOne() // same as doing select * from collection limit 1

```

## exists :

```json
db.c1.find({"word" : {$exists : true}})
```

## Count, Sorting and limiting :

```json
db.collection_name.find({predicate}).limit(no_of_documents)
db.collection_name.find({predicate}).count() //select count(*) from collection_name where predicate
```

```json
db.collection_name.find({predicate}).sort({field_name:  1}) //1 for ascending -1 for descending.
//select * from collection_name where predicate order by field_name 
```

## Or operator :

```json
db.collection_name.find({$or : [{prd1},{prd2}]})
```

## In keyword :

```json
//to check existence in an array for a field.
db.collection_name.find({field_name : {$in : [val1,val2]}})
db.collection_name.find({field_name : {$nin : [val1,val2]}})
```

## and :

```json
db.collection_name.find({$and: [{prd1},{prd2}]})
{$and : [{number : {$exists : true}}, {number : {$nin : [12,13]}} ]}
```

## Querying arrays :

1. to find if a value is in the array.
    
    ```json
    db.collection_name.find({array_field : val}) //just checking the availability of val in the array_field
    ```
    
2. to match the array exactly
    
    ```json
    db.collection_name.find({array_field : [val1,val2,val3 ...]}) //just checking the availability of val in the array_field
    ```
    
3. to see if all the passed values are in the array
    
    ```json
    db.collection.find({arr_field : {$all :  [val1,val2 , ...]})
    ```
    
4. To check if the i’th element value is val :
    
    ```json
    db.collection.find({"arr_field.i" : value})
    ```
    
    ### Deletion of documents :
    
    ```json
    db.collection_name.deleteOne({pred})
    db.collection_name.deleteMany({pred})
    ```
    
    ## Updation of documents :
    
    ```json
    db.collection_name.updateOne({pred}, {what_to_change})
    db.collection_name.updateMany({pred}, {what_to_change})
    ```
    
    ## Set operator :
    
    ```json
    what_to_change -> $set : {field1 : value1 , field2 : value2}
    ```
    
    ## Increment operator :
    
    $inc
    
    ```json
    what_to_change -> $inc: {field1 : increment_amount} //decrement can also be done using negative amounts
    ```
    
    ## Push-Pull :
    
    ```json
    db.collection_name.updateOne({}, {  $pull : {array_field : value_to_remove}  })
    db.collection_name.updateOne({}, {  $push : {array_field : value_to_add}  })
    ```
    
    to add/remove several values : each keyword
    
    ```json
    {$pull : {array_field : {$each : [val1,val2]}}
    ```
    
    ## Size operator :
    
    it is used to query size of array
    
    ```json
    db.collection_name.find({arr_field : {$size : value}})
    ```
    
    ## Type operator :
    
    ```json
    db.collection_name.find({field : {$type : value}}) //10 for null or just use "null"
    ```
    
    ## Not operator :
    
    ```json
    db.col_name.find({field : {$not : {$eq : val}})
    db.col_name.find({field : {$not : {$gt : val}})
    ```