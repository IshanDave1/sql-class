# NOSQL

Not Only SQL

1. Unstructured/Semi structured data handling.
2. BASE as opposed to ACID
3. Join support : Non existent/ minimal (denormalized)
4. Flexible schema/Schema less support, dynamic datatype support

Diff. types of nosql db’s

1. Key - Value Pair : Redis
2. Document db’s : Mongo
3. Wide Column stores : Cassandra, HBase
4. Graph : NEO4j 

| SQL | NOSQL |
| --- | --- |
| Normal form | DB is designed on access patterns |
| Upfront schema design  | Flexible |
| Tabular | Different formats/ JSON(BSON) |
| No scalability  | scalability |
| Fixed data types | flexible data types |

## Mongo DB data model

data model vs data modelling :

data model how data is stored in the db. Modelling is your schema design.

Mongo : 

Database → Database

Table → Collection

Record(row) → Document

Doctor - Patient database for a clinic 

Patient collection :

Documents of each patient.

Documents  : JSON format

Collection: BSON encoding of the JSON document

JSON :  Javascript Object Notation

{

key : value,

key: value,

.

.

.

}

{

name : “siva”,

ph_no: [“988372894” , “938787829873”]

}

{

name : “gauri”,

e_mail : “gaurigg@gmail.com”

}

### Key value pair values :

Key : string

Value : primitive datatype, Arrays, JSON

School database :

Student Collection :

each document is about a prticular student.

{

stu_id : 1,

name : “siva”,

age : 18,

class_result : [{

class_id : 9,

year : 2019,

marks : 420

result : pass

},

{

class_id : 10,

year: 2020,

marks : 470,

result : pass}

,

{

class_id : 11,

year: 2021,

marks : 490,

result : pass,

rank : 8}

]

}

collection names :

1. can’t start with $ , ‘.’ (dot)
2. Can’t be empty string

### Distributed systems :

1. Vertical scaling → adding more to the same monolithic system.
2. Horizontal scaling → multiple systems and distribute the load between all of these. gives you the power of parallalism

### CAP :