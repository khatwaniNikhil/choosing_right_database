1. B-Trees - optimised for reads but limited fast ingestion of data
   1. Update requires random IO and update multiple pages on disk
2. LSM Trees - optimised for writes
   1. MEMTABLE
      1. writes (as they arrive) are batched in memory in Memtable structure
      2. ordered by object tree
      3. implemented as binary balanced tree
    2. SSTABLE
       1. on threshold reach flused to disk into SSTable(key value in sorted sequence) - sequetional IO | FAST
       2. new SSTable becomes the most recent segment of the LSM tree and each SSTable represents chronological subset of incoming change
       3. SSTable are immutable - any object when updated its entry is made into most recent SSTable which supersedes entries of same obj. in past SStables.
       4. to perform delete - add a marker(tombstone) , counterintutive that delete thats extra space
    3. Read request flow
       1. find entry from recent SSTable to older SSTables. since each SStable is sorted lookup is efficient
    5. Compaction tuning is most critical
       1. accumulation of SSTables brings issues
       2. inc. long time to lookup key
       3. disk space is also wasted by tombstones
       4. accumulation process is fast and similar to merge sort
       5. organised into levels of a tree
       6. compaction strategy could be read optimised(size tiered - cassandra or write optimised - rocksdb)
       7. periodic merging and compaction process to cap no of SStables to lookup for read flow
