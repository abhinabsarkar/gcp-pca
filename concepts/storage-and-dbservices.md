# Storage & Database services
* [Storage - Official documentation](https://cloud.google.com/storage/docs/introduction)
* [Storage & Database Services - slides](slides/DataStorageServices.pdf)

## Cloud Storage
* [Cloud Storage - Official documentation](https://cloud.google.com/storage/docs/introduction)

![Alt txt](/images/storage-classes.png)

![Alt txt](/images/storage-access-control.png)

![Alt txt](/images/storage-versioning.png)

![Alt txt](/images/storage-object-change-notification.png)

### Data migration services to Cloud Storage
* [Storage Transfer Service - Official documentation](https://cloud.google.com/storage-transfer-service)
* [Transfer Appliance - Official documentation](https://cloud.google.com/transfer-appliance/docs/4.0)

## File Storage
* [Filestore - Official documentation](https://cloud.google.com/filestore)

## Cloud SQL

[Cloud SQL - Official documentation](https://cloud.google.com/sql/docs/introduction)

* Fully managed DB service - **MySQL, PostgreSQL, Microsoft SQL Server**
* Patched & updated automatically

![alt txt](/images/cloud-sql.png)

### Cloud SQL network connectivity

![alt txt](/images/cloud-sql-connectivity.png)

### Cloud SQL decision tree

![alt text](/images/Cloud-SQL-decision-tree.png)

## Cloud Spanner
[Cloud Spanner - Official documentation](https://cloud.google.com/spanner/docs)

* Spanner is a distributed SQL database management and storage service
* Global transactions, strongly consistent reads, and automatic multi-site replication and failover
* High availability 
    * Multi-regional 99.999%
    * Regional 99.99%
* Used for financial & inventory applications

![alt text](/images/cloud-spanner-characteristics.png)

### Cloud Spanner decision tree

![alt text](/images/cloud-spanner-decision-tree.png)

## Cloud Firestore
[Cloud Firestore - Official documentation](https://cloud.google.com/firestore/docs)

* Firestore is a NoSQL document database
* ACID transactions
* Multi-region replication
* Use cases include mobile, web & IOT applications
* Two modes
    * Datastore mode (new server projects)
    * Native mode (mobile & web apps)

### Cloud Firestore decision tree

![alt text](/images/cloud-firestore-decision-tree.png)

## Cloud Bigtable
[Cloud Bigtable - Official documentation](https://cloud.google.com/bigtable/docs)

* NoSQL big data database service

### Bigtable storage model
[Official documentation](https://cloud.google.com/bigtable/docs/overview#storage-model)

![alt text](/images/bigtable-storage-model.png)

### Bigtable architecture
[Official documentation](https://cloud.google.com/bigtable/docs/overview#architecture)

![alt text](/images/bigtable-architecture.png)

### Decision tree

![alt text](/images/bigtable-decision-tree.png)

## Cloud Memorystore
[Cloud Memorystore - Official documentation](https://cloud.google.com/memorystore/docs/)

* Managed in-memory data store service.
* Caching solution
    * Redis. [Memorystore for Redis - Official documentation](https://cloud.google.com/memorystore/docs/redis)
    * Memcached. [Memorystore for Memcached - Official documentation](https://cloud.google.com/memorystore/docs/memcached)
