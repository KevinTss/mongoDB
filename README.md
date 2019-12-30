# MongoDB

## Mongo Shell

### Installation on Mac

- Install the `tgz` file from [here](https://www.mongodb.com/download-center/community)
- Move the decrompressed file into your user directory
- update your `bash_profile` adding this: `export PATH="/Users/kevintassi/mongodb-macos-x86_64-4.2.2/bin:$PATH"`
- Run `mongo --nodb` to check if it's working
- Nitpick: If your mac blocking the command, just go via finder into your dowloaded file, into `bin` then right click on `mongo` and click on `open with -> terminal`

### Connect to mongo cluster

> Example of connexion command: `mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jsxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m00-student --password m001-mongodb-basics`

> Another simple example: `mongo "mongodb+srv://sandbox-ktwlf.mongodb.net/test" --username m001-student`
> In that case, it will prompt the password input (ex: `m001-mongodb-basics`)

> Or directly with the password: `mongo "mongodb+srv://sandbox-ktwlf.mongodb.net/test" --username m001-student --password m001-mongodb-basics`

### Mongo commands

Til your connected with the command above, you are into the mongo shell command line interface and you can execute theses commands:

- `use {db-name}`: switch to anther database
- `show collections`: show all collections inside the current database
- `db.movies.find().pretty()`: Get all document from the collection `movies` in a proper format
- `quit()`: Exit the shell command line
- `load()`: Execute a file

### CRUD Operation

#### Create

Insert one document
`db.<collection-name>.insertOne({title: "Start Trek II: The Wrath of Khan", year: 1982, imdb: "tt0084726"})`

Insert many documents
`db.<collection-name>.insertMany([{ ... }, { ... }, ...])`

Insert many documents with option
`db.<collection-name>.insertMany([{ ... }, { ... }, ...], { "ordered": false })`

> Ordered false will make the query run after failed is there is one

#### Read

Select document where `year` is equal to `1982`
`db.<collection-name>.find({"year": 1982})`

Select document where `wind.type` is equal to `C`
`db.<collection-name>.find({"wind.type": "C"})`

Select document in an array where `Jeff` is included
`db.<collection-name>.find({"arrayKeyName": "Jeff"})`

Select document in an array where `Jeff` is included at the first position
`db.<collection-name>.find({"arrayKeyName.0": "Jeff"})`

Select document in an array
`db.<collection-name>.find({"writers.0": "Ethan Coen", "writers.1": "Joel Coen"})`

### Operators

Comparison [Doc](https://docs.mongodb.com/manual/reference/operator/aggregation/index.html#comparison-expression-operators)

Where `runtime` is greather than 90
`db.<collection-name>.find({runtime: {$gt: 90}})`

Where `runtime` is greather than 90 & less than 120
`db.<collection-name>.find({runtime: {$gt: 90, $lt: 120}})`

Where `runtime` is greather or equal to 90 & less or equal to 120
`db.<collection-name>.find({runtime: {$gte: 90, $lte: 120}})`

Where `rated` is not equal to `UNRATED` : Will also return document where there is no rated field at all
`db.<collection-name>.find({rated: {$ne: "UNRATED"}})`
