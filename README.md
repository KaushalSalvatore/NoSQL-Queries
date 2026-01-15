### MongoDb And Cassandra Queries
Mongo Db and Cassandra practice queries and Interview Questions.

MongoDB is an open source, document-oriented, NoSql database whose data is stored in an organized format. 
It is scalable and easy to learn, commonly used in modern web and mobile apps, dealing with high volumes of data. 
MongoDB stores data in BSON format, which lets you store JSON like documents efficiently.

// MongoDB Hello World Program

const { MongoClient } = require('mongodb');

// Connection URL
const url = 'mongodb://localhost:27017';
const client = new MongoClient(url);

async function run() {
    try {
        // Connect to the MongoDB server
        await client.connect();

        // Choose the database
        const db = client.db('testdb');

        // Choose the collection
        const collection = db.collection('users');

        // Insert a document
        const result = await collection.insertOne({ name: "John Doe", age: 30 });
        console.log(`Document inserted with _id: ${result.insertedId}`);
    } finally {
        // Close the connection
        await client.close();
    }
}

run().catch(console.error);

Apache Cassandra is an open source, distributed and decentralized/distributed storage system (database), for managing very large amounts of structured data spread out across the world. It provides highly available service with no single point of failure.

Listed below are some of the notable points of Apache Cassandra −

It is scalable, fault-tolerant, and consistent.

It is a column-oriented database.

Its distribution design is based on Amazons Dynamo and its data model on Googles Bigtable.

Created at Facebook, it differs sharply from relational database management systems.

Cassandra implements a Dynamo-style replication model with no single point of failure, but adds a more powerful column family data model.

Cassandra is being used by some of the biggest companies such as Facebook, Twitter, Cisco, Rackspace, ebay, Twitter, Netflix, and more.

cqlsh:> CAPTURE '/home/hadoop/CassandraProgs/Outputfile'
cqlsh:> select * from emp;
cqlsh:> COPY emp (emp_id, emp_city, emp_name, emp_phone,emp_sal) TO myfile;



![Image 1](/images/mongodb-image-01.png)
![Image 2](/images/mongodb-image-02.png)
![Image 3](/images/mongodb-image-03.png)
![Image 4](/images/cassandra-image-01.png)
![Image 5](/images/cassandra-image-02.png)
![Image 6](/images/cassandra-image-03.png)
![Image 7](/images/cassandra-image-04.png)

