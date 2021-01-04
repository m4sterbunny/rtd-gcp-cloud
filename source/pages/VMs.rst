.. _UK_Data_link: https://www.ukdataservice.ac.uk/manage-data/store/security

#################
Virtual Machines
#################

Creating and Linking Virtual Machines
=====================================

Virtual Machines (VMs) are a basic unit of cloud computing.

Setting one up from the console is a simple task: 

.. topic:: Setup a VM from GCP

	GCP > Compute Engine > VM Instances

		+ Name your instance
		+ Choose your zone (or accept default)
		+ Decide you machine type (or accept default)
		+ Decide your OS (Debian 9 Linux is a grand choice)
		+ Modify the firewall to allow HTTP or HTTPS traffic


.. topic:: Launch GCP's `Cloud Shell <cloud-shell.html>`_

	This sets up a VM in the cloud, provisioned with an OS and the GCP SDK. This VM is configured to connect with the VM instances that *you* create.


.. topic:: Setup a VM from the command line

	.. code-block:: bash

		gcloud compute instances create "my-new-vm"

	You will be prompted for a Zone, because the high-level separation of services offered on the GCP are segregated by geographical zones.

	> 46 for us-central1-a

	This minimal command creates a VM with default settings which, if you have not previously setup configurations, will be:

	.. image:: ../images/baseVM.PNG
		:align: center
		

Let's be a little more decisive and prescribe some VM configurations this time. We will create 2 VMs from the command line and connect them.

.. topic:: Create & Connect 2 VMs

	First set your zone as a configuration setting:

	.. code-block:: bash

		gcloud config set compute/zone us-central1-a

	Then create the first VM:

	.. code-block:: bash

		gcloud compute instances create "vm1-zonea" \
		--machine-type "n1-standard-1" \
		--image-project "debian-cloud" \
		--image "debian-9-stretch-v20190213" \
		--subnet "default"
	
	Then set your zone for the next VM:

	.. code-block:: bash

		gcloud config set compute/zone us-central1-b

	... and create the 2nd VM

	.. code-block:: bash

		gcloud compute instances create "vm2-zoneb" \
		--machine-type "n1-standard-1" \
		--image-project "debian-cloud" \
		--image "debian-9-stretch-v20190213" \
		--subnet "default"

	Let's see if our 2 VMs can connect.

	SSH into vm2-zoneb

	GCP> Compute Engine> VM instances> SSH 

	.. code-block:: bash

		ping my-vm-1 -c 3

	NB if you don't set the count for the number of pings then use Ctrl+C to abort the ping command.

Zones matter
============

VM instances are assigned to a zone, that is a sub-region. Zones within a region are better connected that zones between regaions. You have to specify your zone when you set up your VM.

Zone selection may be vital if the following affect you:

	- Cost, not all regions are equal!
	- Data legislation may mean that personal and sensitive data should not be transferred to other countries (see `UK Data protection issues <UK_Data_link_https://www.ukdataservice.ac.uk/manage-data/store/security>`_)
	- Performance, if you users are multi-regional then your service may be improved by running multiple instances in different regions.
		NB performance may be impacted by:
			+ Availability
			+ Latency


Creating your own image
========================

Once you have chosen an image that is close to what you need, either from the GCP itself, its marketplace, by uploading your own custom image, or sourcing from a 3rd party, you may make additional changes to it.

This may include updating or adding libraries or software.

If your project is not simply a one off, then you may use this as a base-image for future projects by creating a snapshot, i.e. by grabbing an image from the boot disk of your VM.


Preemptilble VMs
=================

Where you need a short-lived VM to crunch a specific workload, for example, cyclic reporting you may set up a VM that will persist for up to 24 hours. A preemptible VM may be interupted with only 30s warning, which is why they are cheaper and not suitable for service-delivery.

You can even split workloads across "permanent VMs" and premtiple VMs.


Keen to make something useful?
===============================

It is tough to learn system setup in isolation of normal day to day requirements.

Let's go through a real-world example to bust through the rather abstract ones - e.g., setting up a VM to act as a server for a web page. 

Activate Cloud Shell and use the GCP SDK that it provides.

First, set your zone as a configuration setting:

	.. code-block:: bash

		gcloud config set compute/zone us-central1-a

Setup a new VM:

	.. code-block:: bash

		gcloud compute instances create "my-web-server"
		--machine-type "n1-standard-1" \
		--image-project "debian-cloud" \
		--image "debian-9-stretch-v20190213" \
		--subnet "default"
	

Allow HTTP traffic with

	.. code-block:: bash

		gcloud compute firewall-rules create my-web-server --allow tcp:80

From the GCP console connect to your VM with SSH to set up Apache2 HTTP Server:

GCP> Compute Engine> VM Instances> my-web-server > SSH

	.. code-block:: bash

		sudo apt-get update

	.. code-block:: bash

		sudo apt-get install apache2 php7.0

	.. code-block:: bash

		sudo service apache2 restart

Return to your GCP console and click on the External IP for your VM. This should take you through to your apache landing page.

OR use curl from the SSH connection to your VM's command line:

.. code-block:: bash

	curl http://[Your-External-IP]

Either option should return the apache web server's landing page.

GCP Cloud Shell
================

Google's Cloud Shell is a Linux VM that is pre-loaded with development tools including GCP's SDK. It provides a persistent 5GB home directory and runs on the GCP. Google's Cloud Shell provides command-line access to your GCP resources.

One of the great features of the Cloud Shell environment is that you may store environment variables. This enables you to work with features such as multiple projects without having to return to the console.

..topic:: Storing project id as an environment variable

	If you have multiple projects storing their id with a simple name can speed up work immensely.

		.. code-block:: bash

			gcloud config list | grep project

	Will provide your project id, which may look something like this:

		loony-tunes-251324

	Rather than have to pull the project id every time you want to use it you can shortcode this using:

			.. code-block:: bash

			export Proj-1=loony-tunes-251324


