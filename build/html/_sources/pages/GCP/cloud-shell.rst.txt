

.. _DRY: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

.. _console: https://console.cloud.google.com/

.. toctree::
   :maxdepth: 2
   :caption: Contents:



===========
Cloud Shell
===========

Cloud shell contains GCP's software development kit (:ref:`sdk`) or helper libraries. This massively simplifies interacting with services such as APIs. 

The cloud shell environment sits on a VM spun up in the cloud and accessible by your GCP account. It gives command line access to your projects and their resources according to your IAM settings. 

Unleash the command line interface (CLI) with VMs on your console_.

.. image:: ../../images/shell.PNG
		

Function Trees
===============

Cloud Shell's commands are split by function into:

- gsutil
	- storage
	- bigQuery
- gcloud
	- platform
	- products
	- services (data analytics, ML, networking, APIs)

The gcloud command starts with the group that it manages, e.g. to create an instance using Compute Engine:

.. code-block:: 	

	gcloud compute instances create vm-1



Basic Commands
--------------

Try this from console 

.. code-block:: bash

	gcloud config set account `ACCOUNT`

There are a ton of great tools on this bash- + Google SDK-hosting machine. Here are just a few commands to have a play around with:

- list the active account name with:

.. code-block:: bash
    
	gcloud auth list


- list project ID with:

.. code-block:: bash

	gcloud config list project 

- list organisation ID with:

.. code-block:: 

	gcloud organizations list

Of course, similar information may be pulled using the Compute Engine command:

.. code-block:: bash

	gcloud compute project-info describe

- or see all your accessible projects (this will list the properties in your active SDK configuration):

.. code-block:: bash

	gcloud config list

- pick up the project number directly from gcloud

.. code-block:: bash

	gcloud projects list

A really useful tool is saving environment variables on that VM machine using bash to allow your commands to be written in a kinda DRY_ manner.

- set an environment variable; Hipster Shop version:

.. code-block:: bash

    export HIPSTER_VERSION=1.0.0

Notice that here you are using bash, rather than the gcloud command. NB the SDK does not include bash. It is the Linux VM that is providing bash.

I like this - is there more?
-----------------------------

The big G has, of course, documented the `lot <https://cloud.google.com/sdk/>`_


But here are a few more firm favorites:

.. topic:: display all your functions

	.. code-block:: bash

		gcloud functions list

.. topic:: display all available zones


	.. code-block:: bash

		gcloud compute zones list 

.. topic:: display all available zones in selected region

	.. code-block:: bash

		gcloud compute zones list | grep us-central1
		

Rather than administer each VM directly, you may configure them all through settings.


.. topic:: configure default zone

		.. code-block:: bash

			gcloud config set compute/zone us-central

.. topic:: launch new VM

	.. code-block:: bash

			gcloud compute instances create "my-new-vm"
			--machine-type "n1-standard-1"
			--image-project "debian-cloud"
			--image "debian-9-stretch-v20170918" \
			--subnet "default"


.. topic:: launch a more useful VM

		Being able to talk to the world through your own wee tunnel is very useful. `my-new-vm` could be a lot more fun (and less safe)!

		.. code-block:: bash

			gcloud config set compute/zone us-central

		.. code-block:: bash

			gcloud compute instances create "lamp-1-vm"
			--machine-type "n1-standard-2"
			--image-project "debian-cloud"
			--image "debian-9-stretch-v20170918" \
			--subnet "default"

		Allow HTTP traffic with

		.. code-block:: bash

			gcloud compute firewall-rules create lamp-1-vm
			--allow tcp:80

.. topic:: close cloud shell

		.. code-block:: bash

			exit

SDK
----

GCP's SDK has client libraries for:

- Java
- Pthon
- Node.js
- Ruby
- Go
- .NET

This means that it can be a useful environment in itself. For example, I am using Python's Sphinx to generate this static site from a PC by typing commands in the GCP SDK installed locally on my machine.

