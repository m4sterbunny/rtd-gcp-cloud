#####################
GCP Networking
#####################

Networking Services
###################

On the GCP networks are global and subnets are regional.

.. topic:: Networking on the GCP

	GCP allows for pure cloud networks and linking to on-premises networks.

	The various services include:

	- :ref:`Virtual Private Cloud <virtual private cloud>`
		- Google's network (not public internet)
		- No public IP required to connect across VPC
	- :ref:`Cloud Load Balancing <load-balancing>`
		- distributes workload across regions
		- can load-balance HTTP, HTTPS, TCP/SSL, and UDP
	- Cloud Armor
		- protects against denial-of-service attacks (DDoS)
		- works with HTTP/S load-balancing
	- Cloud Content Delivery Network (CDN)
		- reduces latency with region-based provision
		- ideal for large volumes of static content
	- Cloud Interconnect
		- connects private networks to Google's network
		- provides an SLA
		- alternative is VPN with reduced bandwidth



Virtual Private Cloud
#####################

GCP's Virtual Private Cloud (VPC) supports networking with VM instances, Kubernetes Engine containers, and the App Engine flexible environment. A VPC network is an essential component of linking with VM instances, containers, or App Engine applications. Therefore, each GCP project creates a default network a soon as any of these services are started.

The GCP supports 3 different VPC network types:

- Default
- Auto mode
- Custom mode

The default setup has one subnet per region and pre-set firewall rules. Subnets have a range of IP addresses associated with them and provide private/internal IP addresses for your devices. 

The Auto mode option includes regional IP allocation and one fixed /20 subnet per region. The custom mode does not setup default subnets, it allows full control of IP ranges as well as regional IP allocation.

VPCs have global scope. VPC subnets have regional scope, i.e. they can span the zones that make up a region. 

.. note:: Auto mode networks

	Auto mode networks may be upgraded to Custom mode, but this can not be reverted.

	They include routes, subnets, and firewall rules.

	They are not recommended for production environments. The IP ranges are assigned for you.

	.. code-block:: bash

		ping -c 3 $IP-Address

	Expect positive ping results for internal IPs only if the instance exists on the same network.

	Expect positive ping results for external IPs only if you have a firewall rule that allows ICMP incoming.


You can connect using internal IP addresses from a different network by using options such as VPC peering.


Networks
=========

Networks allow you to isolate systems. Different VMs, for example may exist on different networks. VMs on the same network can communicate using their internal IPs (which are mapped by DNS_), even if they are in different regions. You can ping a VM instance by its name, for example, because VPC networks have an internal DNS service that allows you to address instances by their DNS names rather than their internal IP addresses. This is a very useful feature, because the internal IP address can change when you delete and re-create an instance.

Conversely, even if VMs are in the same region, if they are on different networks they must use their public/external IPs to communicate.

VPC networks function as a distributed firewall, with the firewall rules enforced across the entire network unless set to a specific instance only. 

To view your firewall rules use:

	.. code-block:: bash

		gcloud compute firewall-rules list --sort-by=NETWORK


VPC Networking
==============

By leveraging VPC networks it becomes possible to connect VMs in different zones with subnets. This supports inter project communications. 

A single firewall rule can apply to all VMs on that subnet, even though they are in different zones.

Before creating a shared VPC it's important to assign a member of your organization the `shared VPC admin role`, e.g.:

.. code-block:: 

	gcloud alpha organizations add-iam-policy-binding {org-id} \
	-- member=`user:{gdomain-email}` \
	-- role=`roles/compute.xpnAdmin`

Or all projects can be associated with the VPC from the organisation level:

.. code-block:: 

	gcloud compute shared-vpc associated-projects add {PROJECT_ID} \
	--host-project={HOST_PROJECT_Name}

If there is no organisation setup, then the VPC is managed from `gcloud compute networks` command set.

VPN Networking
---------------

Your VPN (Virtual Private Network) can be connected to securely send network traffic to and from the GCP. 

.. sidebar:: Console

	GCP> Hybrid Connectivity

Create a static IP address to allow your VPN to connect with the VPC network. You will need to specify:

- the network
- region containing the network
- the static IP for the GCP end
- the static IP address of the VPN gateway of your network
- security may be implemented with IKE (Internet Key Exchange)
	- a shared "secret key" can be used
- routing may be:
	- dynamic
	- route-based
	- policy based

Hybrid Infrastructure
---------------------

Enterprise applications can get big fast. A lift and shift into the cloud is no small task, and so most organizations take a hybrid approach – perhaps by taking a microservice approach and starting with cloud microservices.

GCP has a service called Anthos to assist with lift and shift transitions.


Subnets
-------

Every subnet has 4 reserved IP addresses, the first 2 are assigned to the network and the subnet's gateway. The final 2 addresses are also reserved, with the last being used as the broadcast address.

When the GPC sets up a new VPC in auto-subnet mode, then it selects a range of IP addresses for each subnet. 

When creating a custom subnet, be cautious as overly-large subnets can cause conflicts – do not scale a subnet beyond current usage needs. Custom subnets specify the IP range using `CIDR notation`_.

Using Subnets
-------------

To create a subnet from the cloud shell CLI:

To create a custom network:

	.. code-block:: bash

		gcloud compute networks create mynet --subnet-mode=custom

To create a custom subnet:

	.. code-block:: bash

		gcloud compute networks subnets create privatesubnet-us \
		--network=privatenet --region=us-central1 --range=172.16.0.0/24

To create a custom network:

	.. code-block:: bash

		gcloud compute networks create $my-net1 \
		--subnet-mode=custom

To provide this network with firewall rules:

	.. code-block:: bash

		gcloud compute firewall-rules create $my-firewall-custom1 \
		--network my-net1 \
		--allow= tcp, udp, icmp \
		-- source=ranges $IP-range


		gcloud compute firewall-rules create $my-firewall-custom2 \
		--network my-net1 \
		--allow= tcp:22, tcp:3389, icmp 
		

But remember a network is just a collection of subnets. Now you have a network name to group them by, use: 

To create a subnet:

	.. code-block:: bash

		gcloud compute networks create $manage-subnets \
		--region=us-central1 --range=10.130.0.0/20


To create a 2nd subnet:

	.. code-block:: bash

		gcloud compute networks create $manage-subnets \
		--region=us-central1 --range=10.130.0.0/20

To view these networks:

	.. code-block:: bash

		gcloud compute networks list


Instance Isolation
==================

A bastion host acts to isolate an instance for you.

.. image:: ../../images/bastion-host.PNG


DNS
===

A managed zone contains DNS records associated with a DNS name suffix.

In the Public Internet, a public DNS zone hosts www.example.com and DNS records contain specific details about such named items in a zone. For example, an A record maps a hostname to IP addresses in IPv4. AAAA records are used in IPv6 to map names to IPv6 addresses. (CNAME records hold the canonical name, which contains any alias name/s of a domain). 

.. topic:: Resolving name to Public IP

	Public DNS zones are accessible from the Internet. This means that to function, they must provide a name server that will resond to queries, i.e. will provide a valid, routable IP in response to a {domain or hostname} query.

	This is the DNS record held for example.com:

	.. code-block:: 

		example.com
		1 IPV4 RECORD FOUND
		93.184.216.34
		1 IPV6 RECORD FOUND
		2606:2800:220:1:248:1893:25c8:1946

	So, if you want to set up a new website (domain) as an A resource record type (IPv4), that anyone in the world can visit, then you need to specify its IPv4 address on the DNS service that maps domains to IPs for the zone your site exists in. (Although, as there are no public IPv4 records left, you are more likely to setup an IPv6 address).


Private zones provide name services **within** your recognised network. Here IAM matters, only people or services with the accepted permissions, or belonging to recognised access groups, will have their requests accepted and responded to.

DNS security enforces authentication of clients (users and services) attempting to communicate with the DNS service. Such authentication prevents spoofing (a client pretending to be a user or service) and cache poisoning (a malicious update of the DNS server's cache).

A DNS service provides:

- NS name server
- SOA (start of authority record)- provides authoritative information about the zone. 

Then, as devices are added the:

- A (or AAAA) record
- Mail Exchange (MX) records
- CName record

Using DNS on GCP
----------------

To create DNS zones the SDK provides:

.. code-block:: 

	gcloud beta dns managed-zones

.. code-block:: 

	gcloud dns record-sets 

To setup a new, private zone:

.. code-block:: 

	gcloud beta dns managed-zones create {my-new-zone} \
	--descriptions= --dns-name={new-zone.com.} \
	--visibility=private

To add a new record to the name server:

.. code-block:: 

	gcloud dns record-sets transaction start \
	--zone={my-new-zone}

	gcloud dns record-sets transaction add 192.0.2.93 \
	--name={new-zone.com.} --ttl=600 --type=A 
	\ --zone={my-new-zone}

	gcloud dns record-sets transaction execute --zone={my-new-zone}

The final statement executes the write to the record set. The ttl (time to live) is the latency until the update goes into effect.

.. topic:: TTL

	TTL is used by authoritative DNS servers to know when to return to the record to check for updates. Once your IP is stable, GCP suggests setting TTL = 86400, which tells servers across the Internet to check every 24 hours for updates to the record.

CIDR Notation
--------------

When setting up a range of IP addresses in a custom subnet, GCP uses classless inter-domain routing (CDIR). This supports variable-length subnet masking, i.e. the ability to choose the size of a range rather than working with fixed allocations.

The CIDR requires the network address (to id the subnet address), and a host uid. The notation uses /{number} to convey how many bits of an IP address describe the network and how many the host/device.

.. topic:: CIDR /

	192.168.0.0/16 conveys that 16 bits (of a possible 32 bits) are used to describe the subnet and 16 the host addresses (i.e. ~65k possible IP addresses).

The range may be expanded (but not reduced):

.. code-block:: 

	gcloud compute networks subnets expand-ip-range {my-new-subnet} \
	--prefix-length 16


VMs with a Custom Network
--------------------------

A single instance of a VM may also be configured with a custom network.

.. sidebar::

	GCP> Compute Engine> Create Instance 

In the VM customisation form, under the Firewall basics (allow HTTP/HTTPS), is a full set of options> "Management, security, disks, networking ...". The link provides access to individual tabs.

The Networking tab offers "Network Interface" for customisation of the:

- network
- subnetwork
- internal IP address
- external IP address

and more.

Such settings are also available when creating a VM from the CLI:

.. code-block:: 

	gcloud compute instances create {my-vm-name} \
	--subnet{my-existing-subnet} --zone{ZONE-to-APPLY}

Firewalls
----------

Firewalls are applied at the network level and control traffic into and out of VMs.

They act on a port and traffic type, e.g.:

.. code-block:: 

	--allow tcp:80

Firewalls have several configuration options:

- direction in/out (ingress vs egress)
- priority (0=highest, 65535=lowest)
	- default is allow egress to all destinations (65535)
	- default is deny all incoming traffic from any source (e.g. IP=0.0.0.0/0, also 65535)
	- any rule with a higher rating will supersede these fixed rules
- allow/deny 0/1 setting
- target e.g. instance in a network, instance with a specific network tag
- source/destination e.g.:
	- whitelisted IP range/addresses
	- service accounts allowed or denied
	- combination of the above and/or network tags
- protocol and port, if no protocol is specified the rule applies to all
- status enabled/disabled
	- i.e. you could have a firewall ready to trigger if a condition is met, such as a DDoS attack
	- or you can disable a rule for troubleshooting 


.. sidebar:: Logging

	Firewall rules may be associated with logging in Stackdriver. The Flow logs must be configured when setting up the subnet.

.. topic:: Default firewall on a VPC

	The defaults are:
		- incoming traffic allowed for any VM on the same network
		- incoming TCP on port 22 with SSH enabled
		- incoming TCP on port 3389 with Microsoft Remote Desktop Protocol enabled
		- Incoming ICMP for anywhere on the same network

	All the defaults are set to 65534
