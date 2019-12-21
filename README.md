# MongoDB

## Mongo Shell

### Installation on Mac

- Install the `tgz` file from [here](https://www.mongodb.com/download-center/community)
- Move the decrompressed file into your user directory
- update your `bash_profile` adding this: `export PATH="/Users/kevintassi/mongodb-macos-x86_64-4.2.2/bin:$PATH"`
- Run `mongo --nodb` to check if it's working
- Nitpick: If your mac blocking the command, just go via finder into your dowloaded file, into `bin` then right click on `mongo` and click on `open with -> terminal`

### Connect to mongo cluster

`mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jsxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m00-student --password m001-mongodb-basics`

### Mongo commands

- `show collections`: show all collections inside the current database
- `use {db-name}`: switch to anther database
- `db.movies.find().pretty()`: Get all document from the collection `movies` in a proper format
