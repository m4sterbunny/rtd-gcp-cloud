.. _UK_Data_link: https://www.ukdataservice.ac.uk/manage-data/store/security



#################
Virtual Machines
#################

.. topic:: Preemptible VMs

	Use case may be a short-lived VM to crunch a specific workload, for example, cyclic reporting you may set up a VM that will persist for up to 24 hours. A preemptible VM may be interrupted with only 30s warning, which is why they are cheaper and not suitable for service-delivery.
	
	You can even split workloads across "permanent VMs" and preemptible VMs.

.. topic:: Labels

	Labels become vital when you have large numbers of VMs. They can be used to filter the VM list from the console. 

	VMs may also be filtered by:
	- Status
	- Membership of instance group

.. topic:: GPU

	A GPU can take on workload to relieve the CPU. These are usually used for intensitve processes such as visualising data, machine learning, and image processing.

	A GPU is often supported by libraries that will need to be installed. This may be achieved by selecting the relevant boot disk image from the options, or by creating your own.

	Restrictions include:
		- availability of the GPU by zone
		- compatibility with the CPU
		- the instance *must* be set to terminate during maintenance 


Managed Instance Groups
=======================

A group of VMs that run the same configuration are called managed instance groups (MIGs). They are managed as a single entity and created from a group template (default is/was the n1-standard1 image). Instance groups may contain VMs that span a region (i.e. cross multiple zones), providing greater resiliency.

Managed groups are identical VMs that can be set to autoscale and be managed with a load balancer and monitored with a health check. Scaling may be triggered by:

- CPU utilization
- load-balancing metrics
- que-based workload metrics
- request rate
- response latencies
- or, a specified monitoring KPI

Automatic scaling creates dynamic instances based on such metrics. However, you may also specify a minimum number idle instances. This will retain that specified number of instances as resident instances while leaving any additional instances as dynamic.

Unmanaged groups are possible where the VMs have different configurations.

.. note:: warning

	When autoscaling, allow time for VMs to come online, otherwise new VMs may be added that are not required.

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

	OR, using the SSH for VM2 which opens a CLI interface to VM2, you can SSH directly into VM1 (well if your pings works, then you are connected -- right?!)

	.. code-block:: bash

		ssh my-vm-1.us-central1-a

	Try something out, perhaps install a webserver on your VM1 via VM2?

	.. code-block:: bash

		sudo apt-get install nginx-light -y

	Then mess with the landing page

	.. code-block:: bash

		sudo nano /var/www/html/index.nginx-debian.html

	Write something witty in your index and watch it come back at you. Using the SSH from VM2 into VM1. Note that the CLIcur tells you which machine you are connected with (it will have my-vm-1 right there).

	.. code-block:: bash

		curl http://localhost/

Zones matter
============

VM instances are assigned to a zone, that is a sub-region. Zones within a region are better connected that zones between regions. You have to specify your zone when you set up your VM.

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

This may include updating or adding data, libraries, or software.

If your project is not simply a one off, then you may use this as a base-image for future projects by creating a snapshot, i.e. by grabbing an image from the boot disk of your VM.


.. topic:: Restarting VMs

	If a VM is restarted the contents of memory are lost. This means that essential data must be preserved in a persistent disk or Cloud Storage.

.. topic:: Snapshots

	A snapshot is a copy of data held on a persistent disk. This is useful for restoring data. The first snapshot is a full copy of the data. Subsequent snapshots record updated data only.

	If data is held in memory, flush disk buffers **before** creating the snapshot to ensure the storage of all data.

	NB to work with snapshots, the user must have the **Compute Storage Admin** role setup from IAM.

.. topic:: Image v Snapshot

	Both images and snapshots are copies of disk contents. Snapshots make data available on a disk, images are used to create VMs.

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

One of the great features of the Cloud Shell environment is that you may use bash to store environment variables. This enables you to work with features such as multiple projects without having to return to the console.

..topic:: Storing project id as an environment variable

	If you have multiple projects storing their id with a simple name can speed up work immensely.

		.. code-block:: bash

			gcloud config list | grep project

	Will provide your project id, which may look something like this:

		loony-tunes-251324

	Rather than have to pull the project id every time you want to use it you can shortcode this using:

			.. code-block:: bash

			export Proj-1=loony-tunes-251324

			.. code-block:: bash

			echo $Proj-1

			
See more about the :ref:`GCP cloud shell <Cloud Shell>`

.. the above line applies the extension in conf.py to grab file names and headers as a link reference (best practice is to use labels rather than the header itself to allow for headings to change)

.. this ref syntax does not do well inside topics or bullet lists