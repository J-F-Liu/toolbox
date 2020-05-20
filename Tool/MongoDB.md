# MongoDB

## Install

```
pacman -S mongodb mongodb-tools
yay -S robomongo-bin
systemctl start mongodb
```

## Backup Data

```
mongodump -h localhost:27017 -d database -c collection -o ./backup
```

## Restore Data

```
mongorestore -h localhost:27017 -d database -c collection ./backup --drop
```

## MongoDb CLI

```
mongo 10.10.102.12:27017
show dbs //show database names //db.getMongo().getDBs()
show collections //show collections in current database //db.getCollectionNames()
show users //show users in current database //db.getUsers()

db.category.count()
db.category.findOne()
db.category.find({}).sort( { amount: -1 } ).limit(20)
db.category.deleteMany({}) //deletes all documents
db.question.update({},{"$set": {categories: []}},{ multi: true })

db.records.createIndex( { location: 1 } )
```

## Query

[Operators](https://docs.mongodb.com/manual/reference/operator/query/index.html)

```
{$and: [{updatedAt:{$gte: new Date("2018-10-22T19:08:09")}}, {updatedAt:{$lte: new Date("2018-10-22T19:36:41")}}]}
```
