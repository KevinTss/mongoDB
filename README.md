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
- `.count()`: Return the number of document who match the query

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

Select document in an array including specific field
`db.<collection-name>.find({"writers.0": "Ethan Coen", "writers.1": "Joel Coen"}, {title: 1})`
Or excluding setting value to `0`
`db.<collection-name>.find({"writers.0": "Ethan Coen", "writers.1": "Joel Coen"}, {title: 0})`

#### Update

Update one document (the first matching the filter)
`db.<collection-name>.updateOne({ title: "The Martian" }, { $set: { poster: "http://www....png" } })`

Update one document (the first matching the filter)
`const detail = { ... }`
`db.<collection-name>.updateOne({ "imdb.id": 345678 }, { $set: detail }, { upsert: true })`
Will check if the detail already is into the document, if yes, it will replace it.
It avoid me to first fetch the document to manually check before updating.

Update many document (all which match the filter)
`db.<collection-name>.updateMany({ rated: null }, { $unset: { rated: "" } })`
In that case we remove all `rated` filed from all document where rated value is `null`

#### Delete

`db.<collection-name>.deleteOne({ _id: ObjectId('24H3J4J5J43JH2H34H34J3K') }})`
`db.<collection-name>.deleteMany({ reviwer_id: 34567898765 }})`

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

#### Exercice:

> Using the \$in operator, filter the video.movieDetails collection to determine how many movies list "Ethan Coen" or "Joel Coen" among their writers. Your filter should match all movies that list one of the Coen brothers as writers regardless of how many other writers are also listed. Select the number of movies matching this filter from the choices below.

`{writers: {$in: ["Joel Coen", "Ethan Coen"]}}`
`{$or:[{writers: {$in: ["Joel Coen"]}}, {writers: {$in: ["Ethan Coen"]}}]}`

> Connect to our class Atlas cluster from the mongo shell or Compass and view the ships.shipwrecks collection. In this collection, watlev describes the water level at the shipwreck site and depth describes how far below sea level the ship rests. How many documents in the ships.shipwrecks collection match either of the following criteria: watlev equal to "always dry" or depth equal to 0.

`ships.shipwrecks.find({$or: [{watlev: "always dry"}, {depth: 0}]})`

> Connect to our class Atlas cluster from the mongo shell or Compass and view the 100YWeatherSmall.data collection. The sections field in this collection identifies supplementary readings available in a given document by a three-character code. How many documents list: "AG1", "MD1", and "OA1" among the codes in their sections array. Your count should include all documents that include these three codes regardless of what other codes are also listed.

`100YWeatherSmall.data.find({sections: {$all: ["AG1", "MD1", "OA1"]}})`

> Connect to our class Atlas cluster from the mongo shell or Compass and view the 100YWeatherSmall.data collection. How many documents in this collection contain exactly two elements in the sections array field?

`100YWeatherSmall.data.find({sections: {$size: 2}})`

> In the M001 class Atlas cluster you will find a database added just for this week of the course. It is called results. Within this database you will find two collections: surveys and scores. Documents in the results.surveys collection have the following schema.

```
{_id: ObjectId("5964e8e5f0df64e7bc2d7373"),
results: [{product: "abc", score: 10}, {product: "xyz", score: 9}]}
```

The field called results that has an array as its value. This array contains survey results for products and lists the product name and the survey score for each product.

How many documents in the results.surveys collection contain a score of 7 for the product, "abc"?

`{results:{$elemMatch: {product: "abc", score: 7}}}`

> Connect to our class Atlas cluster from the mongo shell or Compass and view the results.scores collection.
> How many documents contain at least one score in the results array that is greater than or equal to 70 and less than 80?

`db.scores.find({results: {$elemMatch: {$gte: 70, $lt: 80}}}).count()`
