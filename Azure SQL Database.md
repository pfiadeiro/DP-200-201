## Azure SQL Database

### Purchasing Models

There are 2 purchasing models:
- **vCore-based** provides a choice between a provisioned compute tier and a serverless compute tier. The provisioned compute tier allows us to choose exact amount of of compute resources that are always provisioned for our workload. With the serverless compute tier, we specify the autoscaling of the compute resources over a configurable compute range.
- - **DTU-based** provides bundled compute and storage packages balanced for common workloads.

## Azure Database Migration Service

Service to move bulk amounts of data. Runs on the Azure platform, as opposed to being a client application like Data Migration Assistant. It's capable of moving large amounts of data quickly and is not dependent upon installation of a client application. Database Migration Service can operate in two modes, offline and online.

In offline mode, no more changes can be made to the source database. Data is migrated, and then applications can begin using the new Azure SQL Database.

In online mode, the source database can remain in use while the bulk of the data is migrated. At the end of the migration, the source system is taken offline momentarily while any final changes to the source are synced to the new Azure SQL Database. At this point, applications can cut over to use the SQL database.

The online option requires creating a SQL Server instance that's based on the Premium price tier.

Part of this service, includes Data Migration Assistant (DMA) which *assists* the service by preparing the target database.

DMA is a client-side tool which can be installed on a Windows workstation and it has 2 major functions:
- Assesses the existing database and identifies any incompatibilities between that database and Azure SQL Database. It then generates a report of the things need fixing before migration.
- Generates the required SQL to migrate the database schema to Azure SQL Database giving the option to run the code or save it to run it later.

Using Data Migration Assistant is not a requirement to use Azure Database Migration Service.

## SQL Elastic Pool

SQL elastic pools are a resource allocation service used to scale and manage the performance and cost of a group of Azure SQL databases. We set the amount of resources available to the pool, add databases to the pool, and set minimum and maximum resource limits for the databases within the pool.

The pool resource requirements are set based on the overall needs of the group. The pool allows the databases within the pool to share the allocated resources. SQL elastic pools are used to manage the budget and performance of multiple SQL databases.

SQL elastic pools are ideal when we have several SQL databases that have a low average utilization, but have infrequent, high utilization spikes. In this scenario, we can allocate enough capacity in the pool to manage the spikes for the group, but the total resources can be less than the sum of all of the peak demand of all of the databases. Since the spikes are infrequent, a spike from one database will be unlikely to impact the capacity of the other databases in the pool.

The general guidance is, if the combined resources you would need for individual databases to meet capacity spikes is more than 1.5 times the capacity required for the elastic pool, then the pool will be cost effective.

## Security

### Firewalls

Azure SQL Database has a built-in firewall that is used to allow and deny network access to both the database server itself, as well as individual databases. Initially, all public access to your Azure SQL Database is blocked by the SQL Database firewall.

Firewall rules are configured at the server and/or database level, and will specifically state which network resources are allowed to establish a connection to the database. Depending on the level, the rules you can apply will be as follows:

**Server-level firewall rules**
- Allow access to Azure services
- IP address rules
- Virtual network rules

**Database-level firewall rules**
- IP address rules

The benefits of database-level rules are their portability. When replicating a database to another server, the database-level rules will be replicated, since they are stored in the database itself.

### Authentication/Authorization

SQL Database supports two types of authentication: SQL authentication and Azure Active Directory (Azure AD) authentication.

SQL authentication method uses a username and password. User accounts can be created in the master database and can be granted permissions in all databases on the server, or they can be created in the database itself (called contained users) and given access to only that database.

This authentication method uses identities managed by Azure AD, and is supported for managed and integrated domains. Use Azure AD authentication (integrated security) whenever possible. 

Authorization refers to what an identity can do within an Azure SQL Database. This is controlled by permissions granted directly to the user account and/or database role memberships.

### TLS Network Encryption

Azure SQL Database enforces Transport Layer Security (TLS) encryption at all times for all connections, which ensures all data is encrypted "in transit" between the database and the client.

### Transparent data encryption

Azure SQL Database protects your data at rest using transparent data encryption (TDE). TDE performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application. Using a database encryption key, transparent data encryption performs real-time I/O encryption and decryption of the data at the page level. Each page is decrypted when it's read into memory and then encrypted before being written to disk.

By default, TDE is enabled for all newly deployed Azure SQL databases.

### Dynamic data masking

By using the dynamic data masking feature of Azure SQL Database, we can limit the data that is displayed to the user. Dynamic data masking is a policy-based security feature that hides the sensitive data in the result set of a query over designated database fields, while the data in the database is not changed.

Data masking rules consist of the column to apply the mask to, and how the data should be masked. We can create our own masking format, or use one of the standard masks

### Azure SQL Database auditing

By enabling auditing, operations that occur on the database are stored for later inspection or to have automated tools analyze them. Auditing is also used for compliance management or understanding how your database is used. Auditing is also required if we wish to use Azure threat detection on our Azure SQL database.

### Advanced Data Security

Advanced Data Security (ADS) provides a set of advanced SQL security capabilities, including data discovery & classification, vulnerability assessment, and Advanced Threat Protection.

- **Data discovery & classification** provides capabilities built into Azure SQL Database for discovering, classifying, labeling & protecting the sensitive data in the databases. It can be used to provide visibility into database classification state, and to track the access to sensitive data within the database and beyond its borders.
- **Vulnerability assessment** is an easy to configure service that can discover, track, and help remediate potential database vulnerabilities. It provides visibility into the security state, and includes actionable steps to resolve security issues, and enhance database fortifications.
- **Advanced Threat Protection** detects anomalous activities indicating unusual and potentially harmful attempts to access or exploit the database. It continuously monitors the database for suspicious activities, and provides immediate security alerts on potential vulnerabilities, SQL injection attacks, and anomalous database access patterns. Advanced Threat Protection alerts provide details of the suspicious activity and recommend action on how to investigate and mitigate the threat.
