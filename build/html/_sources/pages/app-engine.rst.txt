===========
App Engine
===========

App Engine was introduced to run language specific environments. However, App Engine Flexible can run deployments in containers, allowing a microservice and language-independent approach to building apps. With the App Engine Flexible Environment you also have a wider range of languages available, because you can upload your own runtime to run code in a language of your choice. It also supports SSH access to the VM running the environment.

App Engine scales an application automatically in response to the amount of traffic it receives, i.e., billing can drop to zero in the standard environment. This makes App Engine especially suited for applications where the workload is highly variable, such as web apps.

App Engine offers services such as a NoSQL databases, in-memory caching, load balancing, health checks, logging, and user authentication to applications running in it. 


App Engine has native support for languages such as:
- Go
- Python
- Java
- PHP



.. topic:: Application

	Each project can run one application and each app is linked to a region. All resources linked to an app are held in the region specified.

.. topic:: Service

	App:Service is potentially a 1:many relationship, i.e. each App has at least one service.

	A service can have multiple versions thus supporting CICD and previous version maintenance.

	For service think microservice. A service is typically configured to handle a single function.

.. topic:: App Version

	It is the combination of App:Service that defines the "version" of the app. Change the source **or** the Service configuration file and you just made a new App version.

	When a version executes, it creates an instance of the app.



App Engine from the SDK / Cloud Shell
-------------------------------------

Because of the language specificity assumed for most Apps, it is essential to update the environment to accomodate the language you need. For example:

.. code-block:: bash

	gcloud components install app-engine-python


will update the Python library (if required).

Deploy your App
---------------

The files required to run your app must be uploaded into the Cloud Shell environment. The default name for the configuration file is app.yaml, this can be deployed with:

.. code-block:: bash

	gcloud app deploy app.yml

(Although as the file has the default the non-verbose version is gcloud app deploy).

NB this is an environment-dependent command, so you must issue it from the working folder that contains that app.yaml file.

Optional parameters include:

+ --version (provides a tag for version ID)
+ --project (for admins on multiple projects)
+ --no-promote (to override the default deploy with routing traffic)

The App Engine service creates a human-readable link to the app, not just an external IP. This is provided in the output once you run the command.

If you have your own domain, this URL can be updated from the GCP> App Engine >Services. The default is the project name followed by ".appspot.com".

App Engine Scaling
-------------------

App Engine's dynamic instances can add and remove instances according to demand. Autoscaling or dynamic scaling are controlled with key-value pairs in the app.yaml configuration file.

Much like the Kubernetes Engine, deployment is a declarative strategy, you declare the conditions that you want your app to run to and the GCP attempts to match those. Setting up Autoscaling in app.yaml:

.. code-block:: python

	runtime: python 3.7.7
	api_version: 2.8
	threadsafe: true

	handlers:
	- url: /.*
		script: main.app

	automatic_saling:
		target_cpu_utilization: 0.65
		min_instances: 3
		max_instances: 20
		max_concurrent_requests: 30


Basic scaling does not support configuring a minimum number of instances as automatic does. Basic allows for configuration of:

+ idle_time
+ max_instances

Instances may also be configured to "manual scaling" to be permanent, thus removing any cold-start latency issues. These are known as resident instances.

.. code-block:: python

	runtime: python 3.7.7
	api_version: 2.8
	threadsafe: true

	handlers:
	- url: /.*
		script: main.app

	manual_saling:
		instances: 3



Traffic by Version 
-------------------

If you have different versions of the App, say in Arabic and English then you can split traffic to the correct version by:

+ user IP
+ HTTP cookie
+ Randomly (use case would be testing new version, rather than English variant!)

A cookie is best to provide a consistent experience for the user, so that their changing IP does not alter the end experience.

.. code-block:: bash

	gcloud app services set-traffic serV1 --split V1=0.5,v2=0.5 --split-by cookie

This would give you a 50:50 split.

You can also retire a version and redirect its traffic elsewhere with migrate (from the console or the cli).

.. code-block:: bash
	
	gcloud app services set-traffic --migrate V1,V2