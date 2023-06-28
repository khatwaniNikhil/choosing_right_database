# References
https://pub.towardsai.net/exploring-the-nosql-family-49e9f23313ad

# SQL or NOSQL

# NOSQL
## KEY VALUE STORES ( Redis, Riak, Memcache)
1. not designed to be **field-queried**:
   1. value part is opaque to client/consumer of data
   2. query by any field within value/json will lead to full scan across all partitons
2. recommended **for session storage, store user preferences**
   1. supporting  TTL for records
3. with right partition and record key, crud is blazing fast
4. no ACID based txn support
5. can’t access muliple key value pairs in single db request
6. visualise data
   1. spread across partitions(with a partition key based lookup) —Partition(UserID)
   2. within each partition, each record has record key/id and value(json —text, media etc.) —— UserID.Name, UserID.Location, UserID.Height
   3. If a physical partition contains several hot logical partitions (e.g. a set of users that request data frequently)
   4. engine is able to distribute the logical partitions spreading the workload across different clusters. 

## Document oriented database
supports complex and schema less objects 
Documents map to objects in most popular programming languages
can be field queried 
extends the key value model and values are structured format(called as document- JSON, BSON, or XML)
db does indexing of fields of the document/secondary indexes
joins are supported(different from rdbms joins)


## Column oriented DB
Data Warehusing |  analytical applications 
Column-oriented storage improves analytic query performance because it drastically reduces the overall disk I/O requirements
compression scheme selected specifically for the column data type can make storage more optimised

