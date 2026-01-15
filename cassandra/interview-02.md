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

#### Q-6 What is a consistency level ?
```bash
A consistency level (CL) determines how many replicas must acknowledge a read or write before Cassandra considers the operation successful.

Consistency levels help balance:

Consistency
Availability
Latency
Common consistency levels include:

For both reads and writes:

ONE – Only one replica must respond.
TWO, THREE – Two or three replicas required.
QUORUM – Majority of replicas (RF/2 + 1).
ALL – All replicas must respond.
```

#### Q-7 What is tunable consistency ?
```bash
Tunable consistency is Cassandra’s ability to give applications the power to choose the strength of consistency 
for every read or write operation.

Unlike traditional databases (fixed consistency), Cassandra allows developers to tune each query for:

Low latency (weaker consistency)
High accuracy (stronger consistency)
Balance between the two

Example:

If you want faster reads: (Use LOCAL_ONE)
If you want strong consistency: (Use QUORUM or ALL)
```

#### Q-8 Explain eventual consistency ?
```bash
Eventual consistency means that after a write is made, all replicas will eventually converge to the same value, 
but they may not be immediately consistent.

Cassandra follows eventual consistency because:

Writes go to multiple replicas, but one may temporarily be down.
Gossip, read repair, hints, and repairs are used to synchronize data later.
```

#### Q-9 What are tombstones ? 
```bash
A tombstone in Cassandra is a marker indicating that a data item (a row, column, or cell) has been deleted. 
Cassandra does not delete data immediately from SSTables because SSTables are immutable. Instead, it writes a 
tombstone to mark that the data should be ignored.
```

#### Q-10 What is compaction ?
```bash
Compaction is a background process in Cassandra that merges multiple SSTables into fewer, larger SSTables, 
removing deleted data (tombstones) and reducing fragmentation.
```

#### Q-11 What is compression in Cassandra ?
```bash
Compression in Cassandra reduces the size of SSTables stored on disk, decreasing storage requirements and 
improving I/O efficiency.

Key benefits of compression:
Reduces disk space usage (high savings for large datasets)
Improves disk I/O speed (less data to read/write)
Reduces network transfer size during streaming (repairs, bootstrap)
```

#### Q-12 What is a read repair ?
```bash
A read repair is a mechanism in Cassandra that ensures data consistency among replicas during read operations.

Why read repair occurs:
Because Cassandra prioritizes availability and may allow inconsistent replicas temporarily (due to network 
failures, node crashes, or delayed writes).

How read repair works:
A read request is sent to multiple replicas.
Coordinator compares responses.
If mismatch is found (stale values):
The coordinator sends the latest correct version to outdated replicas.
Replicas update themselves automatically.
```

#### Q-13 What is hinted handoff ?
```bash
Hinted handoff is a Cassandra mechanism where the coordinator stores a hint when a replica is down or temporarily 
unavailable to receive a write.

How hinted handoff works:
A write occurs.
One replica is down.
Coordinator stores a "hint" (a small record containing the missed write).
When the replica comes back online:
The coordinator replays the hints to update that node.
```

#### Q-14 What is a coordinator node ?
```bash
A coordinator node is the node that receives a read or write request from the client. It does not need to own 
the data itself.

Coordinator responsibilities:
Determines which replicas hold the data using the partitioner.

For writes:
Sends the write to all required replicas.
Waits for the number of acknowledgments defined by the consistency level.

For reads:
Queries replicas.
Performs read repair if needed.
Returns merged result to the client.
Handles retries and speculative executions.
```

#### Q-15 What is nodetool ?
```bash
nodetool is a powerful command-line administrative tool used to manage and monitor Cassandra nodes. It interacts 
directly with a node through JMX (Java Management Extensions) and provides nearly all operational functionalities 
you need for maintenance, troubleshooting, and cluster inspection.
```

#### Q-16 What is partitioning ?
```bash
Partitioning is the process Cassandra uses to distribute data across multiple nodes in a cluster. It determines 
which node stores which rows based on the value of the partition key.

How partitioning works:

Cassandra applies a partitioner function to the partition key.
This generates a token, which is a large integer.
Each node owns a range of tokens.
The row is stored on the node responsible for the token’s range.

Why partitioning matters:

Ensures even distribution of data
Prevents hotspots
Enables linear scalability
Determines replica placement
```

#### Q-17 Explain Murmur3 partitioner ?
```bash
The Murmur3Partitioner is the default partitioner in Cassandra. It uses the Murmur3 hashing algorithm to hash 
the partition key into a uniform 64-bit token.

Why Murmur3 is used:

Fast — optimized for speed
Uniform distribution — avoids hotspots
Deterministic — same key always maps to the same token
Low collision probability

How it works:

Partition key → Murmur3 hash → 64-bit token
Token → mapped to a node’s token range
Replicated according to replication factor
```

#### Q-18 Explain how Cassandra writes data internally ?
```bash
Step-by-step write path:

1. Client sends a write request to any node (the coordinator node).

2. Coordinator determines replicas using the partitioner + token ring.

3. Write sent to replicas based on consistency level (ONE, QUORUM, etc.).

4. Each replica performs the following:

Append to Commit Log:
The write is appended sequentially to the commit log on disk.
This ensures durability even if the node crashes before flushing to SSTables.

Update Memtable:
The write is applied to the memtable (in-memory sorted structure).
This supports fast in-memory reads and prepares for a flush later.

5. Coordinator waits for required acknowledgments from replicas based on consistency level.

6. Memtable Flush:
When a memtable becomes full:
It is flushed to disk as an immutable SSTable.
Corresponding commit log segments are marked as safe for deletion.

7. Key Concepts:
Writes never read existing data (no read-before-write).
No updates in-place on disk → everything is append-only.
No locking → supports massive concurrency.

This architecture allows Cassandra to handle millions of writes per second with minimal latency.
```

#### Q-19 Explain how Cassandra reads data internally ?
```bash
Detailed internal read flow:

1. Client sends a read request to a coordinator node.
2. Coordinator determines replicas for the partition.
3. Depending on consistency level (ONE, QUORUM, etc.), coordinator sends requests to replicas.
4. Each replica performs the following:

Replica read path (important):

1. Check Row Cache (if enabled)
If present, full row is returned immediately.

2. Check Key Cache (if enabled)
Helps jump directly to the partition index in SSTables.

3. Memtable lookup:
Most recent writes reside in the memtable.

4. SSTable lookup sequence:
Cassandra checks SSTables in order:

    Bloom filter → quick check if partition might exist
    Partition index → locate position in SSTable
    Compression offset map → find compressed block
    Decompress and read data
    Merge column data with other SSTables and memtable

Coordinator merges results:
    Compares timestamps and selects the newest value.
    Performs read repair if replica values differ.
    Returns result to the client.

This multi-layered lookup ensures that Cassandra reads the most recent and consistent version of the data.
```

#### Q-20 What is coordinator node responsibility during reads ?
```bash
The coordinator node orchestrates the entire read operation. It does not need to own the data itself.

Coordinator node responsibilities:

1. Determine replicas
Using partition key → token → replica nodes.

2. Send read requests
Based on consistency level:

ONE → send to 1 replica
QUORUM → send to multiple replicas
Plus an additional replica (to support read repair)

3. Merge data from replicas
Cassandra uses timestamps (last-write-wins)
Coordinator picks newest values

4. Perform read repair
If replicas have inconsistent data
Coordinator pushes latest values to outdated replicas

5. Retry on failure or timeout
If a replica is slow/unresponsive, coordinator triggers speculative retry

6. Return final merged result to client

The coordinator ensures the read is correct, consistent, and within SLA even if some replicas lag or are unavailable.
```