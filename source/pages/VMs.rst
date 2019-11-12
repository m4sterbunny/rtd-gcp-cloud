#################
Virtual Machines
#################

Virtual Machines are a basic unit of cloud computing.

Setting one up from the console is a simple task: 

.. topic:: Setup a VM from GCP

	GCP > Compute Engine > VM Instances

		+ Name your instance
		+ Choose your zone (or accept default)
		+ Decide you machine type (or accept default)
		+ Decide your OS (Debian 9 Linux is a grand choice)
		+ Modify the firewall to allow HTTP or HTTPS traffic


Launch GCP's `Cloud Shell <cloud-shell.html>`_

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
	
	Then set your zone for the next vm:

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






