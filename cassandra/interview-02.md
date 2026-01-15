#### Q-1 What is a table (column family) in Cassandra ?
```bash
A table (often called a column family in earlier Cassandra versions) is where actual data is stored. It is 
conceptually similar to a table in SQL but with fewer restrictions and a flexible schema.

Features of Cassandra tables:

Data is stored using the primary key, which determines location and order.
Each row can have a different set of columns—Cassandra supports a semi-structured model.
Tables are optimized for fast writes and range queries on clustering columns.
Tables automatically generate SSTables and memtables during writes.
```

#### Q-2 What is a clustering key ?
```bash
A clustering key defines the ordering of rows within a partition.
All rows with the same partition key are sorted by clustering columns, stored in sequence for fast 
range queries.

Role of clustering keys:

Enable efficient sorted queries within a partition.
Determine how data is stored on disk inside SSTables.
Allow support for time-series modeling, logs, and chronological ordering.
Provide support for range queries (e.g., >, <, BETWEEN).
```

#### Q-3 What is a wide row ?
```bash
A wide row in Cassandra refers to a row that contains a large number of columns or cells, often stored 
under the same partition key but ordered using clustering keys. Unlike relational databases where rows 
are typically fixed in width, Cassandra allows rows to grow extremely large, sometimes even containing 
millions of cells.

Characteristics of a wide row:
All data shares the same partition key.
Clustering keys define the ordering of data within the row.
Data may span multiple SSTables, but logically it remains a single row.
Enables efficient range queries, e.g., “get last 24 hours of readings.”
```

#### Q-4 Why is Cassandra considered highly available ?
```bash
Apache Cassandra is considered highly available because of its masterless architecture, replication, 
and fault-tolerant design. Cassandra was engineered to remain operational even during node failures or 
network splits.

1. Masterless peer-to-peer architecture
Every node is equal—there is no master, eliminating single points of failure.

2. Replication across multiple nodes
Data is copied (replicated) across nodes and even across multiple data centers, so if one node goes down, 
another replica can serve requests.

3. Tunable consistency
By using consistency levels like ONE or LOCAL_ONE, Cassandra can continue to serve reads and writes even when 
multiple nodes are unavailable.
```

#### Q-5 What is replication in Cassandra ?
```bash
Replication in Cassandra refers to the process of storing multiple copies of data across different nodes 
for reliability, durability, and availability.
```

#### Q-6
```bash
```

#### Q-7
```bash
```

#### Q-8
```bash
```

#### Q-9
```bash
```

#### Q-10
```bash
```

#### Q-11
```bash
```

#### Q-12
```bash
```

#### Q-13
```bash
```

#### Q-14
```bash
```

#### Q-15
```bash
```

#### Q-16
```bash
```

#### Q-17
```bash
```

#### Q-18
```bash
```

#### Q-19
```bash
```

#### Q-20
```bash
```