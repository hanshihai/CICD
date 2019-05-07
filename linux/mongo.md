# Mongo

This is for mongo commands.


### backup and restore

```
mongodump -h localhost -d database -u username -p pwd               :   dump localhost db database into archive folder
mongorestore -h localhost -d database --dir=arch-folder --drop      :   restore the backup db into database
```

### user 

```
show users   : list all users
db.createUser({user: "dbadmin",pwd: "password",roles: [{role: "readWrite", db: "dbname"}]})   : create user on db
db.dropUser("username", {w: "majority", wtimeout: 5000})   : drop user

db.auth("admin", "admin")
```


### common commands

```
mongo   : launch mongo shell
use dbname  : use db
show dbs    : list all dbs
show collections   : list all tables on current db

db.collection.find()   :   select * from tables
db.collection.find({"key":"value"})   : find records by key
db.collection.update({key:value}, {$set {f1:v1, f2:v2}})    : update records by key
db.collection.insert({})   :  insert record
```

### enable mongo server instance with auth
```
mongod --auth
```


### query

$in，$or，$nin

select * from cn where f1 in (a, b, c):
db.cn.find({"f1" : {$in: [a, b, c]}})

select * from cn where f1 not in (a, b, c):
db.cn.find({"f1" : {$nin: [a, b, c]}})

select * from cn where f1 = v1 or f2 = v2:
dc.cn.find({$or : [{f1 : v1},{f2 : v2}]})

select count(*) from cn where f1 = v1:
db.cn.count({f1 : v1})

select distinct from cn where f1 = v1:
db.cn.distinct({f1 : v1})
  
select distinct key from collection:
db.collection.distinct("key")

