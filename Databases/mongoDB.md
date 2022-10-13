<h1 align="center"> MongoDB </h2>

<h2 align="center"> DIFFERENCES & SIMILARITIES BETWEEN MONGODB & SQL </h2>

### Differences			
- MongoDB is a NoSQL language, meaning its philosophy lies in Schemaless formats. Therefore,
    * No JOINS - we can stack data upon data
    * SCHEMALESS - we don't need to adhere to structured schemas, we can add dynamic properties

### Similarities			
- MongoDB is installed as a server that listens to connections made by a language
- MongoDB's installation has a shell to make querys outside of a language
- MongoDB vs MySQL Terminology
    * Database = Database
    * Collections = Tables
    * Documents = Entries  (in MongoDB Documents are in JSON format)

<h2 align="center"> LANGUAGE INFO </h2>

- Drivers:			
    * [MongoDB Docs - Drivers](https://www.mongodb.com/docs/drivers/?_ga=2.122234123.1445355823.1665618408-986227439.1665618408) - Has information on how to use MongoDB with the language of your choice.

<h2 align="center"> BASIC QUERIES </h2>

| Command | Description |
| --- | --- |
| `SHOW DBS` | Shows the Databases |
| `USE someDB` | Uses the named Database (also creates it if it doesn't exist) |
| `db.someTable.insertOne({})` | Inserts a single entry into a specified database table |
| `db.someTable.find()` | find() (w/ no arguments) - shows us the entire contents of the collection |
| Append `.pretty()` | to make JSON returns "prettier" |
