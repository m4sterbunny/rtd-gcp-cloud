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

GCP Storage Options
-------------------

There are varous Cloud Storage options:

	+ Multi-regional
	+ Regional
	+ Nearline
	+ Coldline

Multi-regional is intended for use with data accessed frequently, with regional being the same - with the expectation that this occurs from a particular region.

Nearline is for data that is accessed less than one per month, while Coldline is for data accessed less than annually.

Charges are applied per GB of data stored per month, varying according to the type. Accessing of data is also charged.