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
