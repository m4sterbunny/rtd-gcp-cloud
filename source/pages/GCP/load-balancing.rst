
###############
Load-balancing
###############

Load balancing supports high availability of services. Not all load-balancers are the same! You may want to balance the load for internal services, or for external requests to a service.

Load balancers often work in concert with managed instance groups (MIGs) on the GCP. MIGs support auto-healing, load balancing, autoscaling, and auto-updating. 

Load balancers can perform health checks on individual VMs and take action. The instance group manages the autoscaling, adding or removing servers, or clusters of servers. 

Load balancing allows the use of 2 or more identical clusters. This setup allows you to be resiliant to the loss of a server, or responsive to spikes in traffic.

There are a number of options, e.g.:

	+ Global load balancing of web apps 
		+ HTTP load-balancing
			+ HTTP and HTTPS balanced across backend instances
		+ SSL Proxy load-balancing
			+ terminates connections at the load balancer for any non-HTTP traffic
		+ TCP Proxy load balancer
			+ Terminates TCP sessions at the load balancer which then forwards traffic to services
	+ UDP regional load balancer
			+ Network TCP/UDP to balance based on IP protocol, address, and port (hanndles SSL and TCP traffic not supported by SSL Proxy or TCP Proxy, above)
	+ Internal
		+ Internal TCP/UDP to balance private networks hosting internal VMs
		+ Network TCP/UDP enables balancing based on IP protocol, address, and port

Global load balancing is available using the premium network service tier. Regional load balancing is available on the standard tier. GCP's load balancers are divided into external and internal load balancers. 

External load balancers distribute traffic coming from the Internet to your GCP network. Internal load balancers distribute traffic within your GCP network.

HTTP and HTTPS traffic require *global external* load balancing. TCP traffic can be handled by global external load balancing, external regional load balancing, or internal regional load balancing. UDP traffic can be handled by external regional load balancing or internal regional load balancing.

Using Load Balancing
---------------------

The GCP has a form to assist with setting up load balancing from the console.

Configuration requires setting:

- Single- or multi- regional load balancing
- Public (internet-facing) or private (internal only)
- Include TCP or SSL in the load balancing
- Whether to include a health check against protocol/port
- IP setup
	- name
	- subnet
	- static or ephemeral IP


The form allows you to configure the backend and frontend of the TCP balancer:

- Backend required fields:
	- name
	- region
	- network
	- VMs which will be part of the group
- Frontend required fields:
	- name
	- subnetwork
	- IP configuration (ephemeral v static)
	- port for forwarding traffic on

It also offers a healthcheck.

In the SDK the `gcloud compute` commands control load balancer functions, e.g.:

.. code-block:: 

	gcloud compute forwarding-rules create {my-new-lb} \
	--port=80 --target-pool {my-vm-pool}

traffic can now be routed to any VM in the pool by the load balancer my-new-lb.


.. topic:: Autohealing

	A health check is available on a MIG, you can configure an auto-healing policy. If the auto healer determines that an application isn't responding, the MIG automatically recreates that instance. The health check is configurable to match the service. For example a web application that uses HTTPS needs a health check on port 443 (the standard port for HTTPS traffic).


Load Balancing with K8s
-------------------------

`Kubernetes <kubernetes.html>`_ can be created with service `type: Loadbalancer`. The default is an external load balancer. For an internal load balancer `cloud.google.com/load-balancer-type: Internal` will expose the encryption endpoints running in the pods to applications outside of the immediate cluster on the same VPC network. This internal TCP/UDP load balancer also provides a health check.


