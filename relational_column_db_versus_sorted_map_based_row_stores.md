# References 
1. http://dbmsmusings.blogspot.com/2010/03/distinguishing-two-major-types-of_29.html
2. https://scaleyourapp.com/wide-column-and-column-oriented-databases/

# Group-A (cassandra, HBase, Hypertable, BigTable)
1. Non relational, No SQL interface (no PK, FK, not all rows has same columns)
2. Storage: not every column is stored independently, column families(group of columns) are stored independently
![](https://scaleyourapp.com/wp-content/uploads/2022/05/bigtable-data-storage-min-1536x743.png)
4. diverse set of use cases when specific set of columns are accessed together(stored in single column family)
5. using LSM trees - very high write workloads can be achieved like millions of writes per second
   
# Group-B (Sybase IQ, C-Store, Vertica, VectorWise, MonetDB, ParAccel, and Infobright)
1. Relational + SQL: has PK,FK and all rows has same columns
2. Storage: columnar store - every column is stored independently - all values of single column are together
![](https://image1.slideserve.com/3103565/row-vs-column-stores-n.jpg)
4. suited for analytical workloads/data warehouses -  operations(aggregations, summarizations) on larger set of data instead of single row operations
5. updates rarely, writes in bulk/batch	
