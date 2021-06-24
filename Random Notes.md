- Azure Cosmos DB provides both service-managed key & customer-managed keys for encryption of data.
  
- Currently storing and accessing geospatial data is supported by Cosmos DB SQL API accounts only.
  
- It's possible to configure automatic failover in Cosmos DB

- To optimize read requests costs in Cosmos DB we need to consider (from most efficient to least efficient): point reads (key/value lookup on a single item ID and partition key), query with a filter clause within a single partition key, query without an equality or range filter clause on any property, query without filters.

- When exporting a database, all data stored in encrypted columns is retrieved from the database in the encrypted form (ciphertext) and put into the resulting BACPAC. The resulting BACPAC also contains the metadata for Always Encrypted keys. When the BACPAC is imported into a database, the encrypted data from the BACPAC is loaded into the database and Always Encrypted key metadata is re-created.

- DR strategy for SQL Database Elastic Pools 
  - Databases deployed into 1 elastic pool in one Azure region and management databases deployed as geo-replicated single databases.
  - If an outage occurs in the primary region, the failover group initiates automatic failover of the management database to the DR region.
  - The elastic pool is created with the same configuration as the original pool
  - Geo-restore is used to create copies of tenant databases

- DMV **sys.dm_database_encryption_keys** returns information about the encryption state of a database and its associated database encryption keys.
- DMV **sys.dm_db_resource_stats** returns CPU, I/O and memory consumption of a SQL database
- DMV **sys.dm_exec_requests** returns information about each request that is executing in SQL Server.
- DMV **sys.dm_pdw_exec_requests** returns information about each request currently or recently active in Synapse Analytics
- DMV **sys.dm_db_partition_stats** returns page and row-count information for every partition in the current database


- When adding more databases (shards), in order to redistribute the data to the new databases without disrupting the data integrity, we need to use the **split-merge tool**. The split-merge tool runs as an Azure web service. Some requirements/limitations:
  - Shards need to exist and be registered in the shard map before a split-merge operation on these shards can be performed
  - Schema for all sharded tables and reference tables needs to exist on the target shard prior to any split operation
  - Service does not create tables or any other database objects automatically as part of its operations

- External tables created to be used by Polybase, can't be altered, need to be dropped and recreated

- To enable TDE we need: 1 - create a master key; 2 - create or obtain a certificate protected by the master key; 3 - create a database encryption key and protect it using the certificate; 4 - set the database to use encryption

- Azure Databricks High Concurrency Clusters provide fault isolation by creating an environment for each notebook, isolating them from one another.

- In Azure Streaming Analytics, the **watermark delay** metric can be used to understand if there aren#t enough processing resources to handle the volume of input events.

- In a Hadoop Distributed File System architecture, we have a single **NameNode**, a master server that manages the file system namespace and regulates access to files by clients. There are also a number of **DataNodes**, usually one per node in the cluster, which manage storage attached to the nodes that they run on.

- **Data Sync** uses a hub and spoke topology to synchronize data. The Hub must be an Azure SQL Database, the member databases can be either Azure SQL or instances of SQL Server and the Sync Metadata Database contains the metadata and log for Data Sync.

