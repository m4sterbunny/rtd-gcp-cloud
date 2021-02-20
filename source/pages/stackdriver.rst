.. _VM: VM 

############
Stackdriver
############

Stackdriver is one of those buy-ups that the big G has begun. Along with Quiklabs, they have grabbed a very useful tool. Folk who are waaay past the need for GCP introduction notes may be using Stackdriver to monitor their AWS entities *and* their GCP entities.

Stackdriver is a tool for:
	+ monitoring
	+ logging
	+ debugging

Stackdriver monitors the performance, uptime, and overall health of cloud-based apps. Stackdriver collates metrics, events, and metadata from GCP, AWS, hosted uptime probes, application instrumentation, and a variety of common application components including Cassandra, Nginx, Apache Web Server, Elasticsearch, among others. Stackdriver generates insights via dashboards, charts, and alerts. Stackdriver alerting integrates with Slack, PagerDuty, HipChat, Campfire, and more.

Stackdriver does this via a workspace on GCP. 

.. topic:: Setup Stackdriver

	GCP > Stackdriver 

	Although Stackdriver has its own high-level access from the GCP menu, you will immediately notice that the interface differs.

.. topic:: Set up a VM_ and monitor

	.. code-block:: 