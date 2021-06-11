## Azure Synapse

Synapse SQL offers both serverless and dedicated resource models. For predictable performance and cost, create dedicated SQL pools to reserve processing power for data stored in SQL tables. For unplanned or bursty workloads, use the always-available, serverless SQL endpoint.

Apache Spark for Azure Synapse deeply and seamlessly integrates Apache Spark--the most popular open source big data engine used for data preparation, data engineering, ETL, and machine learning.

Azure Synapse contains the same Data Integration engine and experiences as Azure Data Factory.

Synapse Studio provides a single way for enterprises to build solutions, maintain, and secure all in a single user experience: ingest, explore, prepare, orchestrate, visualize, monitor resources and usage, use RBAC to simplify access to analytics resources, write SQL or Spark code and integrate with enterprise CI/CD processes.

### Azure Synapse Analytics features

- **Workload management** - capability to prioritize the query workloads that take place on the server. It's managed by three related areas:
  - **Workload groups** - enable definition of resources to isolate and reserve resources for its use. It allow the reservation of resources for a group of requests, limits the amount of resources a group of requests can consume, access shared resources based on importance level.
  - **Workload classification** - allows creation of a workload classifier to map queries to a specific classifier. A classifier can define the level of importance of the request so it can be mapped to a specific workload group.
  - **Workload importance** - defined in the *CREATE WORKLOAD CLASSIFIER* command and enables higher priority queries to receive resources ahead of lower priority queries that are in queue.
- **Result-set cache** - in scenarios where the same results are requested on a regular basis, result-set caching can improve the performance of the queries that retrieve these results. When result-set caching is enabled, the results of the query are cached in the SQL pool storage. The result-set cache persists even if SQL pool is paused and resumed later, although the query cache is invalidated and refreshed when the underlying table data or query code changes. To ensure that the cache is fresh, the result cache is evicted on a regular basis on a time-aware least recently used algorithm (TLRU).
- **Materialized views** - A materialized view can pre-compute, store, and maintain data like a table. These views are automatically updated when data in underlying tables are changed. Updating materialized views is a synchronous operation that occurs as soon as the data is changed.

### Massive parallel processing (MPP) concepts

Azure Synapse Analytics separates the computation from the underlying storage which enables one to scale the computing power independently of the data storage. This is done by abstracting the CPU, memory, and IO, and bundling them into units of compute scale called SQL pool.

Azure Synapse Analytics uses a node-based architecture. Applications connect and issue T-SQL commands to a **control node**, which is the single point of entry for the data warehouse. The control node runs the MPP engine which optimizes queries for parallel processing, and then passes operations to **compute nodes** to do their work in parallel. The compute nodes store all user data in Azure Storage and run the parallel queries. The Data Movement Service (DMS) is a system-level internal service that moves data across the nodes as necessary to run queries in parallel and return accurate results.

### Table geometries

Azure Synapse Analytics uses Azure Storage to store data. Because the data is stored and managed by Azure Storage, Azure Synapse Analytics charges separately for the storage consumption. The data itself is sharded into distributions to optimize the performance of the system.

Azure Synapse Analytics supports these sharding patterns:

- **Hash** - can deliver the highest query performance for joins and aggregations on large tables (fact tables, large dimension tables). Azure Synapse Analytics uses a hash function to assign each row to one distribution deterministically. In the table definition, one of the columns is designated as the distribution column. 
- **Round-robin** - distributes data evenly across the nodes, but without any further optimization. A distribution is first chosen at random, and then buffers of rows are assigned to distributions sequentially. It is quick to load data into a round-robin table, but query performance can often be better with hash distributed tables. Joins on round-robin tables require reshuffling data, and this takes additional time. Good for temp/staging tables or when there's no obvious joining key or good candidate column.
- **Replicated** -  fastest query performance for small tables. Caches a full copy on each compute node. Consequently, replicating a table removes the need to transfer data among compute nodes before a join or aggregation. Extra storage is required, and there are additional overheads that are incurred when writing data which make large tables impractical.

### Polybase

PolyBase is a data virtualization feature for SQL Server. Enables a SQL Server instance to query data with T-SQL directly from SQL Server, Oracle, Teradata, MongoDB, Hadoop clusters, Cosmos DB, Azure Blob storage or Azure Data Lake Store without separately installing client connection software. 

A key use case for data virtualization with the PolyBase feature is to allow the data to stay in its original location and format.  We can virtualize the external data through the SQL Server instance, so that it can be queried in place like any other table in SQL Server. This process minimizes the need for ETL processes for data movement. This data virtualization scenario is possible with the use of PolyBase connectors.


