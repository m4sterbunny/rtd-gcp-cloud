#####################
Virtual Private Cloud
#####################

GCP's Virtual Private Cloud (VPC) supports networking with VM instances, Kubernetes Engine containers, and the App Engine flexible environment. A VPC network is an essential component of linking with VM instances, containers, or App Engine applications. Therefore, each GCP project creates a default network a soon as any of these services are started.

The GCP supports 3 different Virtual Private Cloud (VPC) network types:

- Default
- Auto mode
- Custom mode

The default setup has one subnet per region and pre-set firewall rules. The Auto mode option includes regional IP allocation and a fixed /20 subnet per region, it comes with one subnet per region. The custom mode does not setup default subnets, it allows full control of IP ranges as well as regional IP allocation.

.. note::

	Auto mode networks may be upgraded to Custom mode, but this can not be reverted.

=========
Networks
=========

Networks allow you to isolate systems. Different VMs, for example may exist on different networks. VMs on the same network can communicate using their internal IPs (which are actually mapped by DNS), even if they are in different regions.

Conversely, even if VMs are in the same region, if they are on different networks they must use their external IPs to communicate.

VPC networks function as a distributed firewall, with the firewall rules enforced across the entire network unless set to a specific instance only. 

To view your firewall rules use:

.. codeblock:: bash

	gcloud compute firewall-rules list --sort-by=NETWORK

==============
VPN Networking
==============

By leveraging VPN networks it becomes possible to connect VMs in different zones with subnets. A single firewall rule can apply to all VMs on that subnet, even though they are in different zones.

+++++++
Subnets
+++++++

Every subnet has 4 reserved IP addresses, the first 2 are assigned to the network and the subnet's gateway. The final 2 addresses are also reserved, with the last being used as the broadcast address.

Overly large subnets can cause conflicts, be cautious and do not scale a subnet beyond current usage needs.

To create a subnet from the cloud shell cli:

To create a custom network:

.. codeblock:: bash

	gcloud compute networks create mynet --subnet-mode=custom

To create a custom subnet:

.. codeblock:: bash

	gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24

To view your networks:

.. codeblock:: bash

	gcloud compute networks list