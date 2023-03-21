.. toctree::
   :maxdepth: 2

#####################
GCP Data Services
#####################

GCP Database Options
#####################

The GCP provides 6 different database options that fit into 4 categories:

	+ Relational Databases (Cloud SQL and Cloud Spanner)
	+ Non-relational Databases (Cloud Datastore, Cloud Firestore, and Cloud Bigtable)
	+ Object Storage (Cloud Storage)
	+ Warehouse (BigQuery)

Relational databases are restrictive in terms of design. All fields should be identified with the relationships between them established from the outset. Non-relational databases allow for the data types and fields to morph over time, without requiring a redesign of the database.

The Object storage option, for data such as images or other "blobs" is handled by Cloud Storage.

The "Warehouse" storage option is in-fact a relational database, with data accessed using SQL. The reason it sits in its own category is that it is specifically set up to manage huge loads such as those created with data streaming, analytics, and reporting.

Storage & Retrieval
-------------------

Bigtable
========

Cloud Bigtable is a petabyte-scale, fully-managed NoSQL database service for large analytical and operational workloads. Cloud Bigtable can be considered as a persistent hashtable, i.e. each item in the database can be sparsely populated, and is looked up with a single key. It is a wide column database, which means tables can have a large number of columns (and with no schema, rows do not have to provide data for all columns).

A use case would be storing large volumes of streamed sensor data from IoT devices. 

- **NoSQL** Fully-managed NoSQL (think little structure/no schema!) database 
- **Analytical data**
- **Handles heavy read/write**
- **Security** As with cloud storage, data is encrypted in transit and access controlled with IAM. Think Gmail!
- **Capacity** Petabybes+
- **Unit size** 10 + MB
- **Highly scalable**

Configuration

- production or development modes (production = 3 nodes minimum)
- choose persistent disk type (SSD or HDD)
- supports multiple clusters

Using Bigtable
----------------

.. sidebar:: Console

	GCP> Databases> Bigtable

When initiating a multiple cluster provide:

- region/zones 
- number of nodes

Bigtable is typically managed through the SDK. In the case of the SQL databases, the cloud shell connection allows SQL commands. In the case of BigTable, the SDK supports connection once the cbt command is installed:

.. code-block:: 

	gcloud components update
	gcloud components install cbt

Before interacting with Bigtable, identify the instance you are working with (much like you may identify your project).

.. code-block:: 

   echo project = project-id > ~/.cbtrc
   echo instance = quickstart-instance >> ~/.cbtrc

To close down an instance and ensure you do not keep incurring charges:

.. code-block:: 

   cbt deletetable {my-table} \
   cbt deleteinstance {instance-name} \
   rm ~/.cbtrc

.. topic:: Import/Export

	Bigtable does not have an import/export feature.
	A Java application may be used using HBase as an interface. Export of a table then produces a JAR file.

Cloud Datastore
================

Use case, storing structured data for App Engine and Compute Engine applications, for a small app, data storage may be free.

It is a "document database", i.e. the data is held in key-value pairs (much like a JSON schema). With no database schema forcing entities to be alike, it is a great choice for a product catalog, for example, where items have varying characteristics. A book would rarely come with width, height, and depth, but a cupboard would. The lack of a formal data-structure accommodates this. 

Datastore does not support analytics.

- **NoSQL** Fully-managed NoSQL (with SQL-like queries in GQL)
- **Handles transactions**
- **Unit size** 1MB
- **Handles sharding and replication**
- **Highly scalable**
- **Free daily quota**

The Datastore console area accommodates the creation of data queries. Once it is installed on your local SDK, you can leverage the datastore emulator. 

Using Datastore
---------------

.. sidebar:: Console
	
	GCP> Databases> Datastore

Setup requires a name/namespace for the entities (think schema for tables). Each entity has a uid (unique identifier) key.

Backup requires a bucket to hold the file (and of course access permissions set to allow the service/users to edit the bucket). To create backups the Datastore users must have the `datastore.databases.export` permission or the Cloud Datastore Import Export Admin role (which includes `datastore.databases.import` permissions).

Such a user can now run:

.. code-block:: 

	gcloud datastore export -namespaces='{namespace-name}' gs://{bucket-name}

.. topic:: Import/Export

	Notice the use of namespace in the command above? The default is '(default)'. With high-volume streaming, the data is a snapshot of what the namespace holds at the time of the export, this information will be stored as metadata with the file.


Cloud Firestore
================

Use case is storing, querying, and synchronizing data from across distributed systems, e.g. mobile apps. It supports transactional processing and offers redundancy with multiregional replication.

Firestore can act like a front end to Datastore or use its own database.

Using Firestore
---------------

.. sidebar:: Console

	GCP> Databases> Firestore

Cloud Storage
==============

Think movies or large images. Immutable large stuff. Binary or object data.

**Blob Store**
**Capacity** Petabybes+
**Unit size** 5TB

`More on Cloud storage <storage.html>`_


Searchable
-----------

All the following have database schemas, i.e. they are relational SQL databases.

Cloud SQL
==========

Relational database with full SQL transaction processing and consistency, think user-credential verification, customer-order processing.

Scales vertically and horizontally, but limited by the size of the database instance that you choose.

MySQL configuration:

- version
- connection (public or private IP)
- machine type
- automatic backups
- failover replicas
- flags (e.g. read only flag)
- maintenance slots
- labels

Accessible by GCP and external services.

Using Cloud SQL
---------------

.. sidebar:: Console

	GCP> Databases> SQL> 

You will be presented with the SQL choices:

- MySQL
- PostgreSQL 
- SQL Server

Once your instance is created Cloud Shell connects you with:

.. code-block:: bash

	gcloud sql connect {instance-name} -user={optional-username/password}

However, you really should not be passing a password unencrypted like that! If you leave it blank, you will be prompted and a measure of security is applied, you will see nothing as you type.

Success means that a new command-line prompt connected to the SQL instance will open. NB this is a MySQL environment that accepts SQL commands, don't get it confused with the Cloud Shell SDK!

Backups
-------

Both on-demand and automatic backups are supported. These can be managed from the instance's details page or from the Cloud Shell environment:

.. code-block:: bash

	gcloud sql backups create --instance {instance-name}

.. topic:: Imports/Exports


	The console provides a wizard, which provides/accepts data in .csv or SQL files.

.. topic:: CLI Import/Export

	NB if you want to export to a bucket, the SQL instance must have write permissions to the bucket. The instance itself has its own service account name to support IAM:

		.. code-block:: 

			gcloud sql instances describe {instance-name}

		From the output, find the service account's email address.

		Provide this "user" (u) access rights (write permissions, W) to the bucket:

		.. code-block:: 

			gsutil acl ch -u{instance-email-address}:W gs://{bucket-name}

		Now, export data (as .csv) to that bucket:

		.. code-block:: 

			gcloud sql export csv {instance-name} \
			gs://{bucket-name}/{file-name}



Cloud Spanner
==============

Relational database with full SQL transaction processing at a global scale. Large database-driven applications, think job board, ecommerce.

Cloud Spanner provides strong transactional consistency and seamless scaling. 

- **Capacity >2TB** Petabytes+
- **Horizonal scalability**

Configuring:

- instance name and id
- number of nodes
- regional or multiregional 
	this will determine where the nodes are located

Using Cloud Spanner
--------------------

.. sidebar:: Console

	GCP> Databases> Cloud Spanner> Create Instance options include:

After the instance is configured, create the database to be managed by this instance. As with Cloud SQL, as soon as you are interacting with the database you use SQL commands. 

As with other services, such as BigQuery, the GCP provides a query form to use from the console.

.. topic:: Import/Export

	The console provides a form for import/export of data. This is linked to each instance and requires:

	- destination bucket
	- database to export
	- region that applies

	As it is Dataflow that manages this process, there is no gcloud command for import/export.

GCP Big Data Services
=====================

The GCP provides 5 data handling tools:

- Cloud Dataproc
- Cloud Dataflow
- BigQuery
- Cloud Pub/Sub
- Cloud Datalab

BigQuery
--------

BigQuery is a good choice for data analytics warehousing and supports petabytes of data. It supports:

- fast SQL queries against large datasets
- statistical tools
- ML tools

Aces heavy on read/write, able to stream data at 100,000 rows per second.

+ Analytics data warehouse
+ supports SQL
+ load or stream data in
+ free monthly quota
+ data can be held regionally for local compliance
+ people you share data with pay for their own queries

Configuration:

- select a region (not all regions support it)

Pricing, if a customer reaches $40,000 per month, there is a flat-pricing model available.

Using BigQuery
--------------

As a fully-managed service, backups and other admin tasks are automated by the GCP. Tasks that are available include:

- verifying status of a job
- estimating costs of running a query

.. sidebar:: Verify status of a job

	From Big Data> BigQuery> Job History

.. topic:: Estimates

	BigQuery provides an online query editor for SQL queries. In the bottom right of the GUI, the cost estimate is presented.

	Alternatively, 

	.. code-block:: 

		--dry_run

	provides the same.

.. topic:: Create Table

	- source table (optional)
	- create table from (optional, defaults to empty)
		- file format
			- .csv
			- JSON
			- Avro
			- PRC
			- Cloud Datastore Backup
	- destination project
	- dataset name
	- table name
	- table type

	If an external table is selected, the data is accessed from the source table and only metadata stored in BigQuery. This negates the need to import large datasets.

	.. code-block:: 

		bq load \
		--autodetect \
		--source_format={format} {dataset-name}.{table-name} {path-to-source}

	The autodetect switch in the command above asks BigQuery to detect the table's schema from the source file.

.. topic:: Import/Export

	From the console, BigQuery offers two export options, Cloud Storage or Data Studio. File options include:

	- .csv (zip option of Gzip)
	- JSON
	- Avro (zip options deflate & snappy)

	Avro format stores the data schema in a separate file to the data itself.



Cloud Dataproc
==============

Dataproc is a data analyses platform that manages Hadoop-based clusters and jobs on GCP. Managed Hadoop, MapReduce, Spark, PySpark, SparkR, Pig, and Hive service for big data batches.

+ By-second billing
+ Can take advantage of preemtible VMs

Using Cloud Dataproc
--------------------

GCP has a form to create a cluster, specify:

- name of cluster
- region & zone
- nodes (single node for devs), configure machines for master and worker/s
- availability (standard or high)

The cluster handles the master node, you can set the number of workers. To save costs, preemptible VMs may be selected.

Or, via the SDK (check the environment has dataproc setup first):

.. code-block:: 

	gcloud dataproc clusters create {cluster-name} --zone {zone-id}

Once a cluster is running, jobs may be constructed using the GCP's form accessed via the cluster details page. Setup depends on language. e.g.:

- Python for PySpark
- R for SparkR
- Java for those that support it, e.g.:

.. code-block:: 

	gcloud dataproc jobs submit spark \
	--cluster {cluster-name} \
	--jar {java-file-name}

.. topic:: Import/Export

	Custom configuration files may be saved/uploaded:

	.. code-block:: 

		gcloud dataproc clusters export {cluster-name} \
		--destination=gs://{bucket-name}/{file-name.yaml}



Cloud Dataflow
==============

Use cases include fraud detection, point of sale segmentation analyses, personalization experiences.


+ Streak and batch processing for unified and simplified pipelines
+ For data of unpredictable size and rate
+ ETL
+ Orchestration tasks
+ Each step in the pipeline is elastically scaled (i.e. all resources are by demand)


Cloud Pub/Sub
==============

Flexible, scalable enterprise messaging. Operates in a decoupled way. A use case is email or IOT data analyses as the data streams. Another is to use Pub/Sub as an asynchronous interface between systems. For example a retail UI needs to update the invoice stock as customers shop, however, not at the cost of the shopper's experience. A Pub/Sub topic could act as a store of items removed from stock (and added if a shopping cart is emptied). The inventory system would subscribe to the service and receive asynchronous updates. 

+ Supports many-to-many asynchronous messaging
+ Offline support
+ "at least once delivery" 
+ 1 million+ messages per second
+ pairs well with Dataflow
+ push OR pull messages

Using Cloud Pub/Sub
--------------------

.. sidebar:: Console

	GCP> Big Data> Pub/Sub

The console provides a simple form to complete. Two parameters must be defined:

- topic
- subscription
	- name
	- delivery type

A topic receives messages from applications. Applications read messages by creating a subscription. Subscriptions can be push or pull. Pulled implies that the application reads from a topic. Pushed implied that the subscription writes the messages to an endpoint, i.e. the webhook/URL capable of receiving the message must be provided.

Various configurations are possible:

- acknowledgment deadline (10--600 seconds)
- retention period, i.e. the length of time to keep a message that was not delivered

The SDK also supports CLI, e.g.

.. code-block:: bash

	gcloud pubsub topics create {your-topic-name}

.. code-block:: 

	gcloud pubsum subscriptions create \
	--topic {topic-name} {new-subscription-name}

.. topic:: Streaming Data

	It helps to test your message que with a test message:

	.. code-block:: 

		cloud pubsub publish {your-topic-name} \
		--message:{this is a test}

	.. code-block:: 

		cloud subsub subsciptions pull \
		--auto-ack{subscription-name}

.. topic:: Integrating Pub/Sub

	A use case for decoupled Pub/Sub could be a Python App that is triggered by Pub/Sub messages.

	Consider the IAM for this:

	- Assign the roles/run.invoker to your Python App (whether it be in App Engine, Cloud Run if containerised, or apply this to the service account if it runs on a VM).
	- Set up the Pub/Sub subscription to the topic and use the service account to push those messages to.



Cloud Datalab
==============

Data exploration in Python.

+ Notebooks and comments supported e.g. Jupyter Notebook

GCP Machine Learning
=====================

The ML platform is used for:

+ content personalisation
+ fraud detection
+ sentiment analyses

TensorFlow is an opensource ML software library. It links with dedicated hardware for large workloads.

CloudVision API classifies images by category, it can provide the metadata on your image catalog, filter offensive material, or provide sentiment analysis.

Cloud Natural Language API recognises 80+languages. It can transcribe audio into text. 

Cloud Video Intelligence API identifies nouns in video content to make it searchable.

Dataproc supports MLLib for retail recommendation analyses