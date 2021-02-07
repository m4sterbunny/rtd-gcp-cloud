########
Storage
########

.. code-block:: bash

	gsutil mb -l [location]

For example:

.. code-block:: bash

	gsutil mb -l US [projectID-name]



Each object in cloud storage has its own URL. The objects are held in buckets. These object are immutable (unchangable). If you want to edit an image object, for example, you can grab it, edit it, and put it back into storage again - BUT the original will be there unchanged and you will generate a new URL.

GCP cloud storage is like an API that gives you POST, GET, and DELETE but no PUT or PATCH.

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


Buckets
========

As with the rest of GCP location matters. When you create a bucket you set a region that will minimize latency for your typical user/access point.


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


Storage Classes
================

Multi-regional and regional storage are for buckets that are accessed frequently. The cost drops as you enter the long-term storage options.

GCP Storage Options
-------------------

The various Cloud Storage options:

	+ Multi-regional
	+ Regional
	+ Nearline
	+ Coldline

Multi-regional is intended for use with data accessed frequently, with regional being the same - with the expectation that this occurs from a particular region.

Nearline is for data that is accessed less than one per month, while Coldline is for data accessed less than annually.

Charges are applied per GB of data stored per month, varying according to the type. Accessing of data is also charged.

Moving Data
===========

The `gsutil` command is all well and good if you have small requirements that can be handled by your bandwith via the Chrome browser. If you want to schedule batch transfers there is an HTTPS endpoint service that can connect to an upload facility. Up to a pedabyte of data may be transferred this way. Or, you can post your data on a drive (!).

It gets fancy, BigQuery and App Engine can both submit data to cloud storage.