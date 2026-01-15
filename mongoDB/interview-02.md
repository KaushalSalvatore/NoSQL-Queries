#### Q-1 Retrieve All Documents in the Employees Collection and Sort Them by the Length of Their Name in Descending Order ?
```bash
db.employees.aggregate([
    { $addFields: { nameLength: { $strLenCP: "$name" } } },
    { $sort: { nameLength: -1 } },
    { $project: { nameLength: 0 } 
])
```

#### Q-2 Find the Average Salary of Employees in the "Engineering" Department ?
```bash
db.employees.aggregate([
    { $match: { department: "Engineering" } },
    { $group: { _id: null, averageSalary: { $avg: "$salary" } } }
])
```

#### Q-3 Find the Highest and Lowest Salary in the "Engineering" Department ? 
```bash
db.employees.aggregate([
    { $match: { department: "Engineering" } },
    {
        $group: {
            _id: null,
            highestSalary: { $max: "$salary" },
            lowestSalary: { $min: "$salary" }
        }
    }
])
```

#### Q-4 What is map-reduce in Mongodb ?
```bash
Map-reduce is a data processing paradigm that performs operations on large datasets and outputs aggregated 
results. MongoDB offers a built-in mapReduce() function consisting of two stages: map and reduce. 

During the map phase, the function processes each document in the collection and generates key-value pairs. 
These key-value pairs are aggregated in the reduce phase, and operations are performed. 

For example, if you have a collection of text documents, the map function would convert each word into a key 
and assign a value of 1 to it. The reduce function then sums the values of each key to count the occurrences 
of each word across the entire collection.
```

#### Q-5 How do you model a one-to-many relationship in MongoDB ?
```bash
// team details
{
    _id: "Datascience"
   company_name: "DataCamp"
   team_name: "Data leaders"
}

// employee one
{
   name: "John"
   employee_id: "t009456"
email: johnsmith@datacamp.com
}
// employee two
{
  name: "Emily"
  employee_id: "t8068ms"
  email: emilyjones@datacamp.com
}

One-to-many embedded document:
{
    "_id": "Datascience",
    "company_name": "DataCamp",
    "team_name": "Data leaders",
    "employees": [
        {
            "name": "John",
            "employee_id": "t009456",
            "email": "johnsmith@datacamp.com"
        },
        {
            "name": "Emily",
            "employee_id": "t8068ms",
            "email": "emilyjones@datacamp.com"
        }
    ]
}
```

#### Q-6  What are the MongoDB commands for deleting documents ?
```bash
Deletion Methods in MongoDB
deleteOne(): Deletes the first matched document.
deleteMany(): Removes all matching documents.
remove(): Legacy function; use deleteOne() or deleteMany() instead.'

Connect to the database
use myDB;

Delete a single document from 'myCollection'
db.myCollection.deleteOne({ name: "Document1" });

Delete all documents from 'myCollection' with the condition 'age' greater than 25
db.myCollection.deleteMany({ age: { $gt: 25 } });
```

#### Q-7 How do you backup a MongoDB database ?
```bash
You can back up a MongoDB database using tools like `mongodump` or by configuring regular snapshots at 
the file system or cluster level.
```

#### Q-8 Can you explain the complexities involved in MongoDB data sharding ?
```bash
MongoDB data sharding introduces complexities such as choosing the right shard key, managing data distribution, 
ensuring data consistency, and handling shard rebalancing. Handling shard keys and ensuring balanced data 
distribution are key challenges.
```

#### Q-9 How does MongoDB handle concurrency and locking ?
```bash
MongoDB uses document-level locking with the WiredTiger engine. This allows multiple write operations on 
different documents in the same collection without blocking. It improves concurrency and throughput, especially 
in high-traffic systems.
``` 

#### Q-10 How would you perform a text search in MongoDB ?
```bash
First, create a text index on the fields you want to search:
db.articles.createIndex({ title: “text”, body: “text” })

Then, run a search using:
db.articles.find({ $text: { $search: “climate change” } })
```

#### Q-11 What is the default size limit of a document in MongoDB ?
```bash
The default size limit is 16MB per document.
```

#### Q-12 Explain the concept of write concern in MongoDB ? 
```bash
Write concern in MongoDB determines the level of acknowledgment required from the database for write 
operations to be considered successful. It controls the durability and consistency guarantees of write 
operations. Write concern options include w (number of nodes to acknowledge the write), j (journal acknowledgment), 
and wtimeout (timeout for write acknowledgment).

db.collection.insertOne(
   { "name": "Alice" },
   { writeConcern: { w: "majority", j: true, wtimeout: 1000 } }
);
```

#### Q-13 What is the role of the MongoDB Oplog ?
```bash
The MongoDB Oplog (Operation Log) is a special capped collection that records all write operations as they 
occur in a MongoDB replica set. It serves as a mechanism for replication, allowing secondary nodes to replicate 
changes from the primary node in near real-time.
```

#### Q-14 how to Listing databases ? 
```bash
show databases
```

#### Q-15 how to use Logical Operators ?
```bash
$and
$not
$nor
$or

db.testCollection.find({$and:[{age : {$lt : 25}}, {city: "colombo"}]});
```

#### Q-16 What are some common performance issues in MongoDB and how can they be addressed ?
```bash
Common performance issues in MongoDB include slow queries, index misses, and replication lag, which can be 
addressed by optimizing queries, ensuring proper indexing, and monitoring replica sets.
```

#### Q-17 How do you recover data in MongoDB after an unexpected shutdown ?
```bash
Recover data in MongoDB after an unexpected shutdown by using journaling to replay uncommitted operations 
or restoring from a backup. Ensure journaling is enabled for crash recovery support.
```

#### Q-18 Can you explain the concept of data modeling in MongoDB ?
```bash
Data modeling in MongoDB involves organizing documents in collections, deciding between embedded documents 
and references based on access patterns, and indexing to optimize query performance. Effective data modeling 
enhances performance and scalability.
```

#### Q-19 Can you explain the role and configuration of sharding keys in MongoDB?
```bash
Sharding keys in MongoDB determine how data is distributed across shards in a cluster. Choose sharding keys 
based on the query patterns and write distribution, ensuring they provide both high cardinality and even 
distribution to avoid hotspots and ensure scalable performance.
```

#### Q-20 What are the considerations for choosing between single and compound indexes in MongoDB ?
```bash
When choosing between single and compound indexes in MongoDB, consider the query patterns. Use single indexes 
for queries on a single field and compound indexes for queries that involve multiple fields. Ensure the index 
supports the query's sort order to optimize execution.
```