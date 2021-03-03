########
Storage
########


Multiple Storage Options, which to use is determine by your requirements, such as:

+ Data Model  
	+ Do you need a SQL pipeline?
+ Time To Access?
	+ Nanoseconds
	+ Microseconds
	+ Milliseconds

Options include:

+ Persistent disks for VMs
+ Object storage
+ Memorystore
	Redis cache service for caching
+ Archival storage

Cache
-----

While cache is an in-memory store with high-speed access it can't be considered storage as suck, because it does not persist if the machine is shut down.

It is possible to save cache contents to a persistent storage solution at intervals, but the recovery point would not be the same as the shutdown point.


Memorystore
------------

This managed Redis service provides a larger cache which may be configured for high-availability. Memorystore integrates with:

+ Compute Engine
+ App Engine
+ Kubernetes Engine

As with most GCP services, choose region and zone. A basic instance is cheapest, but does not support replicas. The Redis instance will be available on your default network and its IP range may be defined or labels added.

Persistent Disks
----------------

Persistent storage can be associated with your VMs in Compute Engine and Kubernetes Engine. Capacity of up to 64TB. Data is encrypted at rest. Zonal storage or regional storage is available. If zonal is chosen, the data is stored across multiple physical drives. If regional storage is selected, then the date is replicated across zones providing redundancy.

As block storage, such disks can support file systems. The drive is virtual and accessible via your VM. Local SSD (solid state, i.e. high-performance, low latency) drives are an option, but these do not persist through shutdown of the VM.

SSDs are ideal for high-performance, whether with random access or sequential access patterns. They are more expensive than the longer latency, spinning hard disk drive (HDD). 

HDDs can be a better option for storing large amounts of data and undertaking batch processing.

Compare:

NB Read/Write rates are measured per second per GB:

+------------+------------+-------------+-------------------------------------+
| Drive      | Read       | Write       | Notes                               |
+============+============+=============+=====================================+
|SSD         | local ~300 | local ~300  | network attached factor 10 smaller  |
+------------+------------+-------------+-------------------------------------+
| HDD        | 0.75       | 1.5         |                                     |
+------------+------------+-------------+-------------------------------------+

Snapshots of disks can be created as data backups. Once a snapshot disk has been mounted to a new machine, it act like a normal disk, i.e. supports read and write access.

Object Storage
---------------

Cloud Bucket provides simple object storage  for exabyte-volumes of data, or data that needs to be shared widely. No data structures is required, each item is -- an "object". Buckets share a global namespace and, therefore, bucket name must be unique. Using project id as part of the name is a simple method to find a unique key.

The gsutil commands make buckets, like so:

.. code-block:: bash

	gsutil mb -l [location]

For example:

.. code-block:: bash

	gsutil mb -l US [projectID-name]

Cloud Storage Fuse
------------------

There is no file system or folder system with buckets, each object sits at the same level. To create a file structure, Cloud Storage Fuse can be used on Linux O.S. to mount a bucket. This allows file structure to be created from the point of view of the VM/s that possess these mount instructions.

Unique URL
----------

Each object in cloud storage has its own URL. The objects are held in buckets. These object are immutable (unchangeable). If you want to edit an image object, for example, you can overwrite the image held in that bucket, with no impact on the URL.

GCP cloud storage is like an API that gives you POST, GET, and DELETE but no PUT or PATCH. No fragment of a bucket may be updated, it is all or nothing.

.. topic:: Security

	So, how are my objects kept safe? When in transit, objects are transferred over HTTPS. When in storage, it is the IAM that controls who/what can get their hands on the object.

.. topic:: Access

	An Access Control List (ACL) gives fine-grained control over who/what can access the object.

	ACLs have:

	1) Scope = Who/What has access
	2) Permissions = What can be done

.. topic:: Version Control

	A history of modifications can be kept if you turn on object versioning of your bucket/s.

	If you don't turn on versioning then a new file will always overwrite old with no recourse.

.. topic:: Lifecycle Policies

	A set of rules can be applied to buckets. For example, once a bucket reaches a specified age it can be moved to Nearline or Coldline storage. 

	Multiregional & regional objects can > Nearline or Coldline
	Nearline can > Coldline


Storage Classes
================

As with the rest of GCP, location matters. When you create a bucket you set a region that will minimize latency for your typical user/access point. OR for global access, chose multiregional. Multi-regional and regional storage are for buckets that are accessed frequently. The cost drops as you enter the long-term storage options.

GCP Storage Options
-------------------

The various Cloud Storage options:

	+ Multi-regional
	+ Regional
	+ Nearline
		30-day minimum storage
		for data that is accessed less than one per month
	+ Coldline
		90-day minimum storage
		for data accessed less than annually

Multi-regional is intended for use with data accessed frequently, with regional being the same - with the expectation that this occurs from a particular region.

Charges are applied per GB of data stored per month, varying according to the type. Accessing of data is also charged.

Have a Go
---------


.. topic:: Setup a bucket from the console

	GCP> Storage> Browser

	1. Click Create bucket.

	2. Provide a globally unique bucket name (think project id + name)

	3. Click Create.


.. topic:: Setup a bucket from cloud shell

	.. code-block:: bash

		gsutil mb gs://<BUCKET_NAME>


.. topic:: Upload a file via cloud shell

	1. Click the three dots icon in the Cloud Shell toolbar to display further options.

	2. Click Upload file. 

	3. In Cloud Shell's CLI, type ls to confirm that the file was uploaded.

	4. Copy the file into a pre-existing bucket 

	.. code-block:: bash

		gsutil cp [MY_FILE] gs://[BUCKET_NAME]

	NB If your filename has whitespaces, place single quotes around the filename. For example, gsutil cp ‘uploaded file.txt' gs://[BUCKET_NAME]

It is just as simply to copy data from one bucket to another, e.g.:

.. code-block:: bash

	gsutil cp gs://$MY_BUCKET_NAME_1/image.jpg gs://$MY_BUCKET_NAME_2/image.jpg

If you need to verify who has access to a file:

.. code-block:: bash

	gsutil acl get gs://$MY_BUCKET_NAME_1/image.jpg  > acl.txt

	cat acl.txt

To change who has access use:

.. code-block:: bash

	gsutil acl set private
	gs://$MY_BUCKET_NAME_1/image.jpg

A publicly-hosted piece of content would require my more open access, e.g.:

.. code-block:: bash

	gsutil iam ch allUsers:objectViewer gs://$MY_BUCKET_NAME_1

From the Storage options in the GCP, you will be able to pickup the publically-available URL for this item.


Moving Data
===========

The `gsutil` command is all well and good if you have small requirements that can be handled by your bandwith via the Chrome browser. If you want to schedule batch transfers there is an HTTPS endpoint service that can connect to an upload facility. Up to a pedabyte of data may be transferred this way. Or, you can post your data on a drive (!).

It gets fancy, BigQuery and App Engine can both submit data to cloud storage.