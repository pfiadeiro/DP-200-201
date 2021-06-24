## Data Storage

### Types of Data

3 types of data:

- **Structured** - data that adheres to a strict schema, so all of the data has the same fields or properties. Structured data is often stored in database tables with rows and columns with key columns to indicate how one row in a table relates to data in another row of another table. Structured data is straightforward in that it's easy to enter, query, and analyze. However, forcing a consistent structure also means evolution of the data is more difficult as each record has to be updated to conform to the new structure.
- **Semi-structured** - Semi-structured data is less organized than structured data, and is not stored in a relational format, as the fields do not neatly fit into tables, rows, and columns. Semi-structured data contains tags that make the organization and hierarchy of the data apparent - for example, key/value pairs. Semi-structured data is also referred to as non-relational or NoSQL data. The expression and structure of the data in this style is defined by a serialization language (XML, JSON, YAML)
- **Unstructured** - The organization of unstructured data is ambiguous. Unstructured data is often delivered in files, such as photos or videos. The video file itself may have an overall structure and come with semi-structured metadata, but the data that comprises the video itself is unstructured. Therefore, photos, videos, and other similar files are classified as unstructured data.

### Azure Storage Services

- **Blob storage** - object storage solution optimized for storing massive amounts of unstructured data, such as text or binary data. It's ideal for serving images or documents directly to a browser, streaming video and audio, storing data for backup, DR and archiving, storing data for analysis by an on-premises or Azure-hosted service. It supports 3 kind of blobs: *block blobs* used to hold text or binary files u to ~5TB (media files), *page blobs* used primarily as the backing storage for the VHDs and *append blobs* frequently used for logging information.
- **Files** - enables the set up of highly available network file shares that can be accessed using the standard SMB protocol.
- **Queues** - used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.
- **Table storage** - A NoSQL store for schema-less storage of structured data.

### Azure Storage Security Features

- **Encryption at rest** - All data written to Azure Storage is automatically encrypted by Storage Service Encryption (SSE) with a 256-bit Advanced Encryption Standard (AES) cipher, and is FIPS 140-2 compliant. SSE automatically encrypts data when writing it to Azure Storage. When you read data from Azure Storage, Azure Storage decrypts the data before returning it.
- **Encryption in transit** - transport-level security between Azure and the client can be enabled by always using *HTTPS*. This option will also enforce secure transfer over SMB by requiring SMB 3.0 for all file share mounts
- **CORS support** - Azure Storage supports cross-domain access through cross-origin resource sharing (CORS). CORS uses HTTP headers so that a web application at one domain can access resources from a server at a different domain. By using CORS, web apps ensure that they load only authorized content from authorized sources. CORS support is an optional flag.
- **RBAC** - To access data in a storage account, the client makes a request over HTTP or HTTPS. Every request to a secure resource must be authorized. The service ensures that the client has the permissions required to access the data. You can choose from several access options. Arguably, the most flexible option is role-based access. Azure Storage supports Azure Active Directory and role-based access control (RBAC) for both resource management and data operations. 
- **Auditing** - Azure Storage access can be audited by using the built-in Storage Analytics service. It logs every operation in real time and logs can be searched for specific requests.

### Azure Data Lake Storage Gen2

Azure Data Lake Storage combines a file system with a storage platform. It builds on Azure Blob storage capabilities to optimize it specifically for analytics workloads. This integration enables analytics performance, the tiering and data lifecycle management capabilities of Blob storage, and the high-availability, security, and durability capabilities of Azure Storage.

Some of its advantages are:
- **Hadoop compatible access** - it can treat the data as if it's stored in a Hadoop Distributed File System. With this feature, we can store the data in one place and access it through compute technologies including Azure Databricks, Azure HDInsight, and Azure Synapse Analytics without moving the data between environments.
- **Security** - supports access control lists (ACLs) and Portable Operating System Interface (POSIX) permissions. Permissions can be set at a directory level or file level for the data stored within the data lake. 
- **Performance** - organizes the stored data into a hierarchy of directories and subdirectories, much like a file system, for easier navigation. As a result, data processing requires less computational resources, reducing both the time and cost.
- **Data redundancy** - takes advantage of the Azure Blob replication models that provide data redundancy in a single data center with locally redundant storage (LRS), or to a secondary region by using the Geo-redundant storage (GRS) option.

Hierarchical namespaces organize blob data into directories and stores metadata about each directory and the files within it. This structure allows operations, such as directory renames and deletes, to be performed in a single atomic operation. Flat namespaces, by contrast, require several operations proportionate to the number of objects in the structure. Hierarchical namespaces keep the data organized, which yields better storage and retrieval performance for an analytical use case and lowers the cost of analysis.

## Lifecycle Management

Azure Blob Storage lifecycle management offers a rich, rule-based policy for GPv2 and blob storage accounts. The policy can be used to transition data to the appropriate access tiers or expire at the end of the data's lifecycle.

The policy allows:

- Transition blobs from cool to hot immediately if accessed to optimize for performance
- Transition blobs, blob versions, and blob snapshots to a cooler storage tier (hot to cool, hot to archive, or cool to archive) if not accessed or modified for a period of time to optimize for cost
- Delete blobs, blob versions, and blob snapshots at the end of their lifecycles
- Define rules to be run once per day at the storage account level
- Apply rules to containers or a subset of blobs (using name prefixes or blob index tags as filters)

```JSON
{
  "rules": [
    {
      "enabled": true,
      "name": "rulefoo",
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "version": {
            "delete": {
              "daysAfterCreationGreaterThan": 90
            }
          },
          "baseBlob": {
            "tierToCool": {
              "daysAfterModificationGreaterThan": 30
            },
            "tierToArchive": {
              "daysAfterModificationGreaterThan": 90
            },
            "delete": {
              "daysAfterModificationGreaterThan": 2555
            }
          }
        },
        "filters": {
          "blobTypes": [
            "blockBlob"
          ],
          "prefixMatch": [
            "container1/foo"
          ]
        }
      }
    }
  ]
}
```
A lifecycle management policy is a collection of rules in a JSON document. Each rule has a name, a boolean determining whether it's enabled or not, type (Lifecycle) and a definition made up of a filter set and action set.

Rule filters that can be used are *blobTypes* (blockBlob and appendBlob that only supports delete), *prefixMatch* and *blobIndexMatch*.

Rule actions available are *tierToCool*, *enableAutoTierToHotFromCool*, *tierToArchive* and *delete*. The 2nd option isn't available for Snapshot and Version. This is applicable to Base Blob, Snapshot and Version.