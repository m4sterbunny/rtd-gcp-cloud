###############
Load-balancing
###############

Not all load-balancers are the same! You may want to balance the load for internal services, or for external requests to a service.

Load balancing allows the use of 2 or more identical clusters. This setup allows you to be resiliant to the loss of a server, or responsive to spikes in traffic.

Load balancers may be configured to ad or remover servers, or clusters of servers. This is known as autoscaling. 

There are a number of options, e.g.:

	+ Cross-regional load balancing of web apps 
		+ HTTP load-balancing
		+ SSL proxy load-balancing
		+ Global TCP proxy load balancer
		+ UDP regional load balancer

	+ Internal load balancer

Global load balancing is available using the premium network service tier. Regional load balancing is available on the standard tier. GCP's load balancers are divided into external and internal load balancers. 

External load balancers distribute traffic coming from the Internet to your GCP network. Internal load balancers distribute traffic within your GCP network.

HTTP and HTTPS traffic require global external load balancing. TCP traffic can be handled by global external load balancing, external regional load balancing, or internal regional load balancing. UDP traffic can be handled by external regional load balancing or internal regional load balancing.