# References
1. https://pub.towardsai.net/exploring-the-nosql-family-49e9f23313ad
2. https://aws.amazon.com/nosql/key-value/

# CAP Theorem
1. impossible for a distributed data store to simultaneously provide more than two of the three guarantees.
![](https://github.com/khatwaniNikhil/choosing_right_database/blob/main/CAP_theorem.png)

# Tradeoffs based decision
1. Focus on CA(Consistency and Availability) | ACID support | OLTP workloads - **RDBMS**
2. Focus on Availability and Partition Tolerance (AP) | fast and cheap scalability is mandatory | Big Data category | Eventual Consistency is tolerable - **NoSQL**
3. ACID | OLTP workloads | horizontal Scalability - **New SQL**
   1. Examples:
       1. Citus extension for PostgresSQL
       2. YugabyteDB
       3. Vitess
4. ACID | JOINS | Flexible schema | Complex query patterns - PostgreSQL(leverage table inheritance) + ZomboDB plugin based Elasticsearch index


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
4. can be adhoc field queried, flexible indexing - db does indexing of fields of the document/secondary indexes
5. extends the key value model and values are structured format(called as document- JSON, BSON, or XML)
6. joins are supported(different from rdbms joins)

## Column oriented DB(Cassandra, HBase)
1. Data Warehousing |  analytical applications
2. Column-oriented storage improves analytic query performance because it drastically reduces the overall disk I/O requirements
3. compression scheme selected specifically for the column data type can make storage more optimised

