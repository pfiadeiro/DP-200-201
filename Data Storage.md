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

