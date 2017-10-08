Notes from "Learning MongoDB" by Kirsten Hunter (Lynda.com)
==================================================

Download (ubuntu way)
=====================
From: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/  
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6  
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list  
sudo apt-get update  
sudo apt-get install -y mongodb-org  
```

Run server
=====================
Logs: /var/lib/mongodb, /var/log/mongodb  
Specify data path from /etc/mongod.conf => defaults to /var/lib/mongodb, this is ok for now  

sudo service mongod start  
tail /var/log/mongodb/mongod.log  

sudo service mongod stop  
sudo service mongod restart  

Mongo Shell
=====================
```
$ /usr/bin/mongo  
$ db  
$ show dbs  
$ use admin  
$ show collections  

$ use test_db  
$ db.test_collection.insert({"key1":"val1","key2":"val2"})  
$ db.test_collection.find()  
$ db.test_collection.insert({"key1":"val3","key2":"val4", "key_array": [ "arr1", "arr2", "arr3" ]})  
$ db.test_collection.find().count()  
$ db.test_collection.find({"key1":"val3"})  
```

Find:
-----
"Projections" - only get a subset of the fields or document back (like columns in relational db)
    
- you get \_id back by default, so need id:0 to undo that
- .pretty -> make lots of new lines to make it more readable
- .sort -> 1 is ascending, -1 is descending
- .limit ie first x records only
- .skip ie skip a bunch of records
- Range search: instead of 1 or -1 for the search object, create an object and specify like key1:{$lte:2,$gte1}
```
    db.test_collection.find({}, {key1:1})
    db.test_collection.find({}, {key1:1,_id:0})
    db.test_collection.find({}, {key1:1,_id:0}).pretty().sort({key1:-1}).limit(2)
```

findOne instead of find -> get an object back

Use javascript + findOne to do joins and more advanced queries, and put it in a js object:
```
var moviename = "xxx"
var movieobj = db.movies.findOne({_id:moviename})
var personArray = db.people.find({movies:moviename})
personArray.foreach(person){
    movieobj.cast.push(person)
}
```

Aggregate:
----------
Use $group operator for aggregation, examples:
```
db.tours.aggregate([$group:{_id: '$tourPackage', count: {$sum: 1}}])
db.tours.aggregate([$group:{_id: '$tourOrganizer.organizerName', count: {$sum: 1}}])
db.tours.aggregate([$group:{_id: '$tourOrganizer.organizerName', count: {$sum: 1}}])
db.tours.aggregate([$group:{_id: '$tourPackage', average: {$avg: '$tourPrice'},  count: {$sum: 1}}])
```

Update: 
---------
```
db.test_collection.update({"key1":"val1"},  
    { $set: {"key2":"val8"}}  
)  
```

Delete
---------
```
db.test_collection.remove({"key1":"val1"})  
```

CRUD is: 
* insert
* find
* update
* remove
* drop - drop the whole collection + indexes

Bulk copy for mongo: 
=====================
mongoimport --collection <collection name> --db <dbname> --file <filename.json> --jsonArray
(I guess apt put it in /usr/bin)

Explain:
=====================
```
db.xxx.find.().explain("executionStats")
```

Indexes:
====================
Max 64 indexes per collection  
Unique indexes ONLY enforce at insert time, so you MUST create index before inserts (ie on empty collection)

Simple Index:
-------------
```
db.xxx.createIndex({key1:1})
db.xxx.createIndex({key1:1},{unique:true})
```

Text Index:
-----------
Only one text index per collection is allowed! But you can make that one text index cover multiple text fields
```
db.xxx.createIndex({key1:"text",key2:"text"})

# To search the text index:
db.xxx.find({$text:{$search:"key1"}}).pretty()
```
Text searches will have a "score" for how well it matches:
```
db.xxx.find({$text:{$search:"key1"}},{score:{$meta:"textScore"}}).pretty().sort({score:{$meta:"textScore"}})
```
Regex Search:
```
db.xxx.find({textFieldName:{$regex:/backpack/i}},{someField:1,_id:0})
```

Composite:
----------
```
db.xxx.createIndex({key1:1,key2:1})
```

Query Tuning:
=============

# TODO - Mongo via node.js
==========================
Mongoose - schema for node in mongo?
