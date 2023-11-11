# References
1. https://www.pythian.com/blog/why-column-stores
2. https://pub.towardsai.net/exploring-the-nosql-family-49e9f23313ad
3. https://aws.amazon.com/nosql/key-value/
4. https://medium.com/helpdotcom/techtalk-tackling-flexible-schema-in-relational-databases-ddcf0c095615
5. https://inviqa.com/blog/understanding-eav-data-model-and-when-use-it
6. https://www.mongodb.com/compare/cassandra-vs-mongodb
7. http://dbmsmusings.blogspot.com/2010/03/distinguishing-two-major-types-of_29.html
8. https://www.pythian.com/blog/cassandra-use-cases


# MongoDB versus sycalla db
https://www.youtube.com/watch?v=ch1vRQBiXtI&t=8s
Favours flexibility over performance
Sycalla favours consistent perforamce
1. Feature set as evolved over time:
   1. WiredTiger Engine based data storage (3.2)
   2. change data streams
   3. ability to apply schema to collection (3.6)
   4. distributed acid transaction support
2. Core -
   1. C++
   2. XFS filesystem
   3. unbounded cores for mongodb processing
   4. field level encryption support, 
3. Cluster topology
   1. replica set has single primary node for writes
   2. in case of sharding - multiple replica sets gets created and coordinator, router nodes
   3. for sharding to work - background process continously moves data/chunks, this workload and business access patterns
      workload are not bounded to any cpu cores and thus have big performance impact.
4. Ops perspetive
   1. Sharding cluster takeaways mgmt. -  TCO and Ops rating are ba
   2. Single replica set based topology with vertical scaling master for writes is not bad(another way could be to migrate to sycalla db)
5. Dev perspective
   1. Geospatial queries
   2. text search
   3. aggregation pipelines
   4. graph queries
   5. change streams
6. Strengths of Mongodb
   1. support query over flexible schemas like webtracking data
   
# MongoDB versus Cassandra

| Data Modelling | general purpose document store | partitioned row store<br>(store a column family in a row-by-row fashion)<br>(columns are part of the data and not part of the schema |
| --- | --- | --- |
|  | BSON | Log-structured merge trees(LSM) |
|  | <br>databases -> collections -> documents -> columns -> values | Keyspace -> column families  -> rows -> ordered columns -> (column name + column value + timestamp) |
|  | flexibility around no of columns in any document | flexibility around no of columns in any document |
|  | Each collection visualised as List<JSON> | each column family represented as nested sorted map<br>Map<RowKey, SortedMap<ColumnKey, ColumnValue>> |
|  |  |  |
| IO Operation | complete document gets picked | group of columns in a column family gets picked |
|  |  |  |
| Query patterns | query patterns can evolve as indexes can be added later for different columns | need upfront clarity around columns to be access together and data modelling should be done accordingly |
|  | write flow<br>read from disk -> modify->update to disk | write flow - suitable for write heavy workloads<br>write in memtable -> flush at regular interval to SSTable/disk(seq IO - append mode) |
|  |  |  |
| Choose criteria | data is more dynamic, <br>query patterns can evolve<br>CP(write flow availability sacrificed when leader is down or disconcected due to n/w partition) | 1. Writes exceed reads by a large margin. <br>2. Immutable append only data with almost no updates/deletes.<br>3. reads are very targeted(by primary key)<br>4. Data is rarely updated and when updates are made they are idempotent.<br>5. Data can be partitioned for linear scaling.<br>6. There is no need for joins or aggregates.<br>7. No ACID or locks use cases<br><br>Realworld use cases<br>1. tracking: sensors data, health tracker, weather, iot of cars trucks, ecommerce order/shipment status changes. |
|  |  |  |
| example - use cases | blogging website | sensors data, health tracker, weather, iot of cars trucks, ecommerce order/shipment status changes. |

<br />
<br />

# Tradeoffs based decision
1. ACID support | OLTP workloads | tunable for CA,CP,AP based on needs(https://stackoverflow.com/questions/29663645/why-are-rdbms-considered-available-ca-for-cap-theorem) - **RDBMS**    
3. Focus on Availability and Partition Tolerance (AP) | fast and cheap scalability is mandatory | Big Data category | Eventual Consistency is tolerable - **NoSQL**
4. ACID | OLTP workloads | horizontal Scalability - **New SQL**
   1. Examples:
       1. Citus extension for PostgresSQL
       2. YugabyteDB
       3. Vitess
5. ACID | JOINS | Flexible schema | Complex query patterns - PostgreSQL(leverage table inheritance) + ZomboDB plugin based Elasticsearch index

<br />
<br />

# NOSQL
## KEY VALUE STORES (Redis, Riak, Memcache, Amazon Dynamodb(supports json based document store data model)
1. not designed to be **field-queried**:
   1. value part is opaque to client/consumer of data - query by any field within value/json will lead to full scan across all partitons
2. recommended for **Session storage | Store user preferences |  Shopping cart**
   1. supporting  TTL for records
3. with right partition and record key, crud is blazing fast
4. no ACID based txn support
5. can’t access muliple key value pairs in single db request
6. visualise data
   1. spread across partitions(with a partition key based lookup) — Partition(UserID)
   2. within each partition, each record has record key/id and value(json —text, media etc.) —— UserID.Name, UserID.Location, UserID.Height
   3. If a physical partition contains several hot logical partitions (e.g. a set of users that request data frequently), engine is able to distribute the logical partitions spreading the workload across different clusters. 

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

## Graph Databases
