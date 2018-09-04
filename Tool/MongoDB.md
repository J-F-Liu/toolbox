# MongoDB

## Install
```
pacman -S mongodb mongodb-tools
packer -S robomongo-bin
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
mongo
show dbs //show database names //db.getMongo().getDBs()
show collections //show collections in current database //db.getCollectionNames()
show users //show users in current database //db.getUsers()

db.category.count()
db.category.findOne()
db.category.find({})
db.category.deleteMany({}) //deletes all documents
db.question.update({},{"$set": {categories: []}},{ multi: true })
```
