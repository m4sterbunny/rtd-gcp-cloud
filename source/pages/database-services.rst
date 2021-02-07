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

**NoSQL** Fully-managed NoSQL (think little structure!) database 

**Analytical data**
**Handles heavy read/write**

**Security** As with cloud storage, data is encrypted in transit and access controlled with IAM. Think G-mail!

**Capacity** Petabybes+

**Unit size** 10 + MB

Cloud Datastore
================

e.g., stores structured data for App engine applications.


**NoSQL** Fully-managed NoSQL (with SQL-like queries)
**Handles transactions**
**Unit size** 1MB
**Handles sharding and replication**
**Free daily quota**


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

Scales vertically and horizontally.

MySQL and POSTgreSQL.

Accessible by GCP and external services.

Cloud Spanner
==============
Full SQL transaction processing. Large database-driven applications, think job board, ecommerce.

**Capacity >2TB** Petabybes+
**Horizonal scalability**

BigQuery
========

Umm Storage? Processing? Processing? Aces heavy read/write.

