#### Q-1 How does MongoDB differ from relational databases ?
```bash
Data Model:

MongoDB: Document-oriented, stores data in flexible JSON/BSON documents.
Relational DB: Table-based, stores data in rows and columns with a fixed schema.
Schema:

MongoDB: Schema-less; documents in a collection can have different structures.
Relational DB: Schema must be defined upfront; all rows follow the same structure.
Relationships:

MongoDB: Uses embedding or referencing; no JOINs required.
Relational DB: Uses foreign keys and JOINs to establish relationships.
Scalability:

MongoDB: Horizontally scalable using sharding.
Relational DB: Typically vertically scalable; horizontal scaling is complex.
Querying:

MongoDB: Uses a rich query language for documents, supports nested structures and arrays.
Relational DB: Uses SQL for structured queries across tables.
```

#### Q-2 Explain the concept of a document and a collection in MongoDB ? 
```bash
Document:
A document is the basic unit of data in MongoDB.
It is a JSON-like object (stored as BSON internally) containing key-value pairs.
Each document can have different fields and nested structures.


Collection:
A collection is a group of documents in MongoDB.
It is similar to a table in relational databases, but schema-less.
All documents in a collection are stored together and can be queried collectively.
Example: A users collection may contain multiple documents like the one above.
```

#### Q-3 How does MongoDB store data internally ?
```bash
MongoDB stores data in BSON (Binary JSON) format, which is optimized for speed and traversing. Collections 
are stored in databases as files on disk using the WiredTiger storage engine.
```

#### Q-4 Explain BSON and Its Significance in MongoDB ? 
```bash
BSON (Binary JSON) is a binary-encoded serialization format used by MongoDB to store documents. BSON extends 
JSON by adding support for data types such as dates and binary data and it is designed to be efficient in both 
storage space and scan speed.
```

#### Q-5 How to Create a New Database and Collection in MongoDB ?
```bash
use mydatabase
db.createCollection("mycollection")
```

#### Q-6 How Does MongoDB Ensure High Availability and Scalability ?
```bash
MongoDB ensures high availability and scalability through its features like replica sets and sharding.
Replica sets provide redundancy and failover capabilities by ensuring that data is always available.
Sharding distributes data across multiple servers, enabling horizontal scalability to handle large volumes of 
data and high traffic loads.
```

#### Q-7 How do you insert data into a collection ? 
```bash
You can use the insertOne() or insertMany() methods.
db.users.insertOne({ name: “John”, age: 28 })
```

#### Q-8 Explain the Concept of Replica Sets in MongoDB ? 
```bash
A replica set in MongoDB is a group of mongod instances that maintain the same data set.
A replica set consists of a primary node and multiple secondary nodes.
The primary node receives all write operations while secondary nodes replicate the primary's data and 
can serve read operations.
If the primary node fails, an automatic election process selects a new primary to maintain high availability.
```

#### Q-9 What is Sharding, and How Does It Work in MongoDB ?
```bash
Sharding is a method for distributing data across multiple servers in MongoDB. It allows for horizontal 
scaling by splitting large datasets into smaller, more manageable pieces called shards.

1. Each shard is a separate database that holds a portion of the data.
2. MongoDB automatically balances data and load across shards, ensuring efficient data distribution and 
high performance.
```

#### Q-10 Explain the Basic Syntax of MongoDB CRUD Operations ?
```bash
Create: db.collection.insertOne({ name: "Alice", age: 25 })
Read: db.collection.find({ name: "Alice" })
Update: db.collection.updateOne({ name: "Alice" }, { $set: { age: 26 } })
Delete: db.collection.deleteOne({ name: "Alice" })
```

#### Q-11 How to Perform Data Import and Export in MongoDB ?
```bash
Import Data:
mongoimport --db mydatabase --collection mycollection --file data.json

Export Data:
mongoexport --db mydatabase --collection mycollection --out data.json
```

#### Q-12 Describe the Aggregation Framework in MongoDB ? 
```bash
The Aggregation Framework in MongoDB is a powerful tool for performing data processing and transformation on 
documents within a collection.
operation on the data, such as filtering, grouping, sorting, reshaping and computing aggregations.

db.sales.aggregate([
  { $match: { status: "completed" } },  // Filter completed sales
  { $group: { _id: "$product", totalSales: { $sum: "$amount" } } },  // Group by product and sum the sales amount
  { $sort: { totalSales: -1 } }  // Sort by total sales in descending order
])
```

#### Q-13 How to Handle Transactions in MongoDB ?
```bash
MongoDB supports multi-document ACID transactions by allowing us to perform a series of read and write operations 
across multiple documents and collections in a transaction. This ensures data consistency and integrity. To use 
transactions we typically start a session, begin a transaction, perform the operations and then commit or abort 
the transaction.

try {
  db.collection1.insertOne({ name: "Alice" }, { session });
  db.collection2.insertOne({ name: "Bob" }, { session });
  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
} finally {
  session.endSession();
}
```

#### Q-14 How to Optimize MongoDB Queries for Performance ?
```bash
Indexes: Create appropriate indexes to support query patterns.
Query Projections: Use projections to return only necessary fields.
Index Hinting: Use index hints to force the query optimizer to use a specific index.
Query Analysis: Use the explain() method to analyze query execution plans and identify bottlenecks.
Aggregation Pipeline: Optimize the aggregation pipeline stages to minimize data processing and improve 
efficiency.
```

#### Q-15 Explain the Concept of Horizontal Scalability and Its Implementation in MongoDB ? 
```bash
Horizontal Scalability in MongoDB refers to the ability to add more servers to distribute the load and data. 
This is achieved through sharding, where data is partitioned across multiple shards.
Each shard is a replica set that holds a subset of the data. Sharding allows MongoDB to handle large datasets 
and high-throughput operations by distributing the workload.
```

#### Q-16 Find all Employees Who Work in the "Engineering" Department ?
```bash
db.employees.find({ department: "Engineering" })
```

#### Q-17 Find the Employee with the Highest Salary ? 
```bash
db.employees.find().sort({ salary: -1 }).limit(1)
```

#### Q-18 Update the Salary of "John Doe" to 90000 ? 
```bash
db.employees.updateOne({ name: "John Doe" }, { $set: { salary: 90000 } })
```

#### Q-19 Count the Number of Employees in Each Department ? 
```bash
db.employees.aggregate([
    { $group: { _id: "$department", count: { $sum: 1 } } }
])
```

#### Q-20 Add a New Field Bonus to All Employees in the "Engineering" Department with a Value of 5000 ? 
```bash
db.employees.updateMany({ department: "Engineering" }, { $set: { bonus: 5000 } })
```