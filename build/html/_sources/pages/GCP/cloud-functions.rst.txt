==================
Cloud Functions
==================

Cloud Functions supports the running of single-purpose functions that behave in isolation. The default time out is 1 minute and the max possible is 9 minutes.

Cloud Functions is suited for logic tests and small, defined tasks. Have a photosharing tool? Cloud Functions can manage the code to check a user is uploading an image file and pass the T/F answer back to the app as part of the logic before the upload is accepted.

In this example, an *Event*, an upload to Cloud Storage, *Triggers* the *Function* to create an *Event* and send a message back to the app system, say via Cloud Pub/Sub's messaging service.

The integration options are myriad:

+ Trigger an Event using HTTP verbs (the normal API POST, GET, PUT, DELETE) and OPTIONS
+ Forward Stackdriver logs to Pub/Sub and trigger a response from there
+ Upload, Delete, Update of metadata, and Archive in Cloud Storage
+ Firebase database Events

Runtime Environments
--------------------

Currently include:

+ Python 3
+ Node.js 6 & 8

The supported languages (at time of writing) include:

+ Python
+ Go
+ JS / Node.js

Using Cloud Functions on the GCP
================================

.. sidebar:: Console

	GCP> Compute> Cloud Functions
	
Requirements:

1. Cloud Functions Engine API
2. Your SDK / Cloud Shell may need its environment updated to support your runtime environment

Settings include:

+ Function Name
	for use on GCP, different to the name of the function to be called!
+ Name 
	of the function to execute
+ Memory allocation
+ Trigger
	+ HTTP / Cloud Pub/Sub / Cloud Storage
+ Event type
+ Location of code
+ Runtime environment
+ Source Code


Cloud Functions from the CLI
-----------------------------

The command to deploy is:

.. code-block:: bash

	gcloud functions deploy {function-name}

The command accepts 3 parameters:

+ runtime
	+ i.e. Python 3.7 or the Node version
+ trigger-resource
	+ defines the bucket name if a Cloud Bucket is associated with this task
+ trigger-event
	+ defines the event type

So the following function:

.. code-block:: bash

	gcloud functions deploy {function-name}
		--runtime python37
		--trigger-resource {bucket-name}
		--trigger-event google.storage.object.delete


is triggered by a delete occuring on a named bucket.

As Cloud Function is simply yet another GCP API, it merrily provides a payload upon successful completion. This is presented as a log message.

Integration with Cloud Pub/Sub
------------------------------

If the trigger is to be Cloud Pub/Sub, this can be selected from the console (or coded in the CLI). You will then be prompted to identify the name of the Pub/Sub topic, if this name does not exist it will be automatically created.

