.. toctree::
   :maxdepth: 2

===================
Deployment Manager
===================

Deployment manager can make implementing a complex deployment configuration as easy as "shopping" in GCP's Cloud Launcher market place. Deployment manager simplifies the deployment process by having pre-defined declarative configurations written in YAML.

Having your configurations recorded in one file simplifies the reuse of common deployment paradigms, e.g. MIGs, load-balanced, auto-scaled instance groups. You can also deploy multiple resources simultaneously.

With deployment manager, you can preview an update you want to make before committing any changes. The Deployment Manager service previews the configuration by expanding the full configuration and creating "shell" resources. Deployment Manager does not actually set up any actual resources when you preview a configuration. This allows you to see the deployment before committing to it.

Google best practice is to script infrastructure and deploy using Deployment Manager. Each deployment file can have full version control and mangement through tools such as Git.

Resources
---------

A YAML configuration for deployment starts with resources:

- type of resource (i.e. name of compute engine instance e.g. compute.v1.instance)
- name internal name of the resource
- properties, i.e. key-value pairs describing settings such as:
	- machine type
	- disks
	- network interfaces

Templates
----------

Templates can exists in another file and be called into the main configuration file. Such templates may be written in:

- Python
- Jinja2
- templating language

GCP recommends Python.

You can launch a deployment template with:

.. code-block:: bash

	gcloud deployment-manager deployments create {my-new-deployment} \
	--mydeploy-file.yaml

