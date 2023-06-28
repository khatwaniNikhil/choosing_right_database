# References
1. https://pub.towardsai.net/exploring-the-nosql-family-49e9f23313ad
2. https://aws.amazon.com/nosql/key-value/

# CAP Theorem
1. impossible for a distributed data store to simultaneously provide more than two of the three guarantees.
![](https://github.com/khatwaniNikhil/choosing_right_database/blob/main/CAP_theorem.png)

# SQL or NOSQL
1. RDBMS
   1. focus on CA(Consistency and Availability)
   2. ACID support
   3. Some SQL databases are able to combine horizontal sharding/scaling and distributed queries
      1. Citus extension for PostgresSQL
      2. YugabyteDB
      3. Vitess
   
3. NOSQL
   1. fast and cheap scalability is mandatory | Big Data category
   2. sacrificing Consistency in favor of Availability and Partition Tolerance (AP) for horizontal partitioning and speed
      1. Eventual Consistency - eventually, all the nodes within the cluster will contain the same data version.
   3. Some Document-Value NoSQL databases(Azure Table Storage, MongoDB) allow for ACID-like transactions as long as performed within the same collection

# NOSQL
## KEY VALUE STORES (Redis, Riak, Memcache, Amazon Dynamodb(supports json based document store data model)
1. not designed to be **field-queried**:
   1. value part is opaque to client/consumer of data
   2. query by any field within value/json will lead to full scan across all partitons
2. recommended for **Session storage | Store user preferences |  Shopping cart**
   1. supporting  TTL for records
3. with right partition and record key, crud is blazing fast
4. no ACID based txn support
5. can’t access muliple key value pairs in single db request
6. visualise data
   1. spread across partitions(with a partition key based lookup) — Partition(UserID)
   2. within each partition, each record has record key/id and value(json —text, media etc.) —— UserID.Name, UserID.Location, UserID.Height
   3. If a physical partition contains several hot logical partitions (e.g. a set of users that request data frequently)
   4. engine is able to distribute the logical partitions spreading the workload across different clusters. 

## Document oriented database(MongoDB, Amazon DocumentDB)
1. recommended for **catalogs | user profiles | and content management systems**
2. supports JSON-like documents - flexible, semistructured, and hierarchical nature
3. Documents map to objects in application layer 
4. can be adhoc field queried, flexible indexin
extends the key value model and values are structured format(called as document- JSON, BSON, or XML)
db does indexing of fields of the document/secondary indexes
joins are supported(different from rdbms joins)


## Column oriented DB(Cassandra, HBase)
1. Data Warehousing |  analytical applications
2. Column-oriented storage improves analytic query performance because it drastically reduces the overall disk I/O requirements
3. compression scheme selected specifically for the column data type can make storage more optimised

