#################
Virtual Machines
################

Virtual Machines are a basic unit of cloud computing.

Setting one up from the console is a simple task: 

.. topic:: Setup a VM from GCP

	GCP > Compute Engine > VM Instances

		+ Name your instance
		+ Choose your zone (or accept default)
		+ Decide you machine type (or accept default)
		+ Decide your OS (Debian 9 Linux is a grand choice)
		+ Modify the firewall to allow HTTP or HTTPS traffic

.. topic:: Setup a VM from the command line

	Launch GCP's Cloud Shell

	This sets up a VM in the cloud provisioned with an OS and the GCP SDK. This VM is configured to connect with the VM instances that *you* create.

	

