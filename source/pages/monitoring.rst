.. toctree::
   :maxdepth: 2


=============
Monitoring
=============


Monitoring assists with system performance management, regulatory compliance, and billing analyses.

Stackdriver
===========

Stackdriver is one of those buy-ups that the big G has begun. Along with Quiklabs, they have grabbed a very useful tool. Folk who are waaay past the need for GCP introduction notes may be using Stackdriver to monitor their AWS entities *and* their GCP entities.

Stackdriver is a tool for:
	+ monitoring
	+ logging
	+ debugging

Stackdriver coordinates collection of performance metrics, event logs, and event data from multiple sources including GCP resources.

.. topic:: Example Predefined Metrics

	+------------+------------+-------------+
	|Service     | Metric Monitored         | 
	+============+============+=============+
	|VM          |CPU % utilised            |
	+------------+------------+-------------+
	|BigQuery    |Time for executions       |
	+------------+------------+-------------+
	|Functions   |Execution count	        |
	+------------+------------+-------------+


Using Stackdriver
------------------

.. sidebar:: Console

		GCP> Monitoring

	   

To monitor a VM, Stackdriver must have an agent installed on that VM. If you have a Linux machine, then using curl from bash you can call and install the agent via the SSH connection to that VM:

.. code-block:: bash

	curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh 
	sudo bash add-monitoring-agent-repo.sh && \
	sudo apt-get update

You will have to select a version number and install the latest as per the `GCP's directions <https://cloud.google.com/monitoring/agent/installation#agent-install-debian-ubuntu>`_.

The agent needs a Stackdriver workspace to send data to. Stackdriver can be setup via the GCP console. Each project can have a space or projects may be added to another project's space.

Stackdriver can be set up to email reports on a cycle or to send alterts. These setting are held in a policy. The policy configuration can use labels given to VMs, or general properties such as zone, region, project ID. If no labels were applied to VMs, instance ID can be applied to pick up individual VMs.

The Stackdriver report is highly customisable:
	
	- Aligning, grouping data, e.g. data every 20s can be reported as av. per min
	- Reducing, consolidating data into max, min, mean, standard deviation

Alerts can be set for these reports, e.g. CPU usage >85% for 2 minutes. Alert channels include:

	- email
	- Slack / HipChat 
	- GCP console
	- PagerDuty
	- Campfire

.. topic:: Custom Metrics

	If none of the predefined variables measures what you need, then create custom metrics.

	There are 2 APIs that GCP recommends:
		- Stackdriver's own 
		- OpenCensus

	A custom metric must be programmed to call the monitoring API in a language such as Python, etc.

Configuring Log Sinks
----------------------

The default data-retention period in Stackdriver is 30 days. To assist with long-term storage or advanced analytics, Cloud Storage (with lifecycle management) and BigQuery are useful tools.

Stackdriver> Exports form assists with exporting data to a "sink". You need to configure the sink:

	- name
	- service (BigQuery, Storage/Bucket, Pub/Sub, custom)
	- destination

When setting up a GCP service, the form with allow export to an existing location or create a new service item.


Cloud Trace
============

.. sidebar:: Console

	GCP> Trace

Cloud Trace is a service that collects latency data from a GCP app. This assists with performance monitoring, e.g. to identify bottle-necks.

Traces are only generated when applications are programmed to call Cloud Trace. As with Stackdriver, the reports are customisable.


Cloud Debugger
==============

.. sidebar:: Console 

	GCP> Debugger

Cloud Debug assists developers to inspect the state of a running instance. It can take snapshots of the state of an app in App Engine and can also be enabled on CE and GKE.

Using Debugger
--------------

The GCP console form provides a drop-down of deployments such as an App Engine app. From here the code can be examined line by line, and any **line of interest** can be clicked to create a snapshot of the instance when that line of code executes. Alternatively a log point may be placed at that line.