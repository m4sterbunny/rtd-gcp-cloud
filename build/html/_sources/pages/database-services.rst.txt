#####################
GCP Data Services
#####################

GCP Database Options
#####################

The GCP provides 6 different database options that fit into 4 categories:

	+ Relational Databases (Cloud SQL and Cloud Spanner)
	+ Non-relational Databases (Cloud Datastore and Cloud Bigtable)
	+ Object Storage (Cloud Storage)
	+ Warehouse (Big Query)

Relational databases are restrictive in terms of design. All fields should be identified with the relationships between them established from the outset. Non-relational databases allow for the data types and fields to morph over time, without requiring a redesign of the database.

The Object storage option, for data such as images or other "blobs" is handled by Cloud Storage.

The "Warehouse" storage option is infact a relational database, with data accessed using SQL. The reason it sits in its own category is that it is specifically set up to manage huge loads such as those created with data streaming, analytics, and reporting.

Storage & Retrival
-------------------

BigTable
========

Cloud Bigtable can be considered as a persistent hashtable, i.e. each item in the database can be sparsely populated, and is looked up with a single key. 

A use case would be storing large volumes of streamed sensor data from IoT devices. 

- **NoSQL** Fully-managed NoSQL (think little structure/no schema!) database 
- **Analytical data**
- **Handles heavy read/write**
- **Security** As with cloud storage, data is encrypted in transit and access controlled with IAM. Think G-mail!
- **Capacity** Petabybes+
- **Unit size** 10 + MB
- **Highly scalable**

Cloud Datastore
================

Use case, is storing structured data for App Engine and Compute Engine applications, e.g. a small app whose application's data storage could be free.


- **NoSQL** Fully-managed NoSQL (with SQL-like queries)
- **Handles transactions**
- **Unit size** 1MB
- **Handles sharding and replication**
- **Highly scalable**
- **Free daily quota**


Cloud Storage
==============

Think movies or large images. Immutable large stuff. Binary or object data.

**Blob Store**
**Capacity** Petabybes+
**Unit size** 5TB

Searchable
-----------

All the following have database schemas, i.e., they are relational SQL databases.

Cloud SQL
==========

Full SQL transaction processing: think user-credential verification, customer order processing.

Scales vertically and horizontally, but limited by the size of the database instance that you choose.

MySQL and POSTgreSQL.

Accessible by GCP and external services.

Cloud Spanner
==============
Full SQL transaction processing at a global scale. Large database-driven applications, think job board, ecommerce.

Cloud Spanner provides strong transactional consistency and seamless scaling. 

**Capacity >2TB** Petabytes+
**Horizonal scalability**



GCP Big Data Services
#####################

The GCP provides 5 data handling tools:

- Cloud Dataproc
- Cloud Dataflow
- BigQuery
- Cloud Pub/Sun
- Cloud Datalab

BigQuery
========

Big Query is a good choice for data analytics warehousing. It supports fast SQL queries against large datasets. Aces heavy on read/write, able to stream data at 100,000 rows per second.

+ Analytics data warehouse
+ supports SQL
+ load or stream data in
+ free monthly quota
+ data can be held regionally for local compliance
+ People you share data with pay for their own queries

Cloud Dataproc
==============

Managed Hadoop, MapReduce, Spark, Pig, and Hive service.

+ By-second billing
+ Can take advantage of preemtible VMs

Cloud Dataflow
==============

Use cases include fraud detection, point of sale segmentation analyses, personalization experiences.


+ Streak and batch processing for unified and simplified pipelines
+ For data of unpredictable size and rate
+ ETL
+ Orchestration tasks
+ Each step in the pipeline is elastically scaled (i.e. all resources are by demand)


Cloud Pub/Sub
=============

Flexible, scalable enterprise messaging. Operates in a decoupled way. A use case is email or IOT data analyses as the data streams.

+ Supports many-to-many asynchronous messaging
+ Offline support
+ "at least once delivery" 
+ 1 million+ messages per second
+ pairs well with Dataflow
+ push OR pull messages

Cloud Datalab
=============

Data exploration in Python.

+ Notebooks and comments supported e.g. Jupyter Notebook

GCP Machine Learning
=====================

The ML platform is used for:
+ content personalisation
+ fraud detection
+ sentiment analyses

TensorFlow is an opensource software library. It links with dedicated hardware for large workloads.

CloudVision API classifies images by category, it can provide the metadata on your image catalog, filter offensive material, or provide sentiment analysis.

Cloud Natural Language API recognises 80+languages. It can transcribe audio into text. 

Cloud Video Intelligence API identifies nouns in video content to make it searcable.