

==================
Accounts & Billing
==================

.. topic:: today's Princess Bride extended quote (adapted)

	You told me to go back to the beginning. ... So it's the beginning, and I'm staying till I understand ...

GCP's Identity and Access Management (IAM) policies are a key component to issues around accounts and billing. 

.. topic:: Go to:

	GCP > IAM & admin


For all billing and organisation services.

GCP Resource Hierarchy
======================

.. topic:: Principles to Secure By
	
	InfoSec and data-management best practice dictate: *only assign access to those that **must** have it*.

	OR - *the principle of least privilege*

Constraints are restrictions on services. By constraining roles, the GCP determines, using lists or a boolean switch, who can do what - where.

The GCP applies the following resource hierarchy: 
+ Organisation
+ Folder
+ Project

Google offers Identity as a Service (IDaaS). By using a google identity through G Suite, you can create an organisation in your GCP hierarchy. If your company does not use G Suite, you can use Cloud Identity, Google’s other IDaaS offering.

.. topic:: Go to:

	GCP > IAM & admin > Identity & Organisation

All users are assigned project creater and billing account creator by defailt- i.e. every user may initiate a service that creates costs.

Futher reponsibilites may be assigned, for example a super-user position would have the Organisation Administrator IAM role assigned. These users can then assign that role to other selected users.

The constraints that GCP supports include:

+ *allow* a set of values
+ *deny* a set of values
+ deny this and all the children of this
+ allow all allowed values
+ deny all values

.. sidebar:: constraints rule

	To deny access to serial ports on VMs, you can set these constraints:

	``compute.disableSerialPortAccess to TRUE``

	If you set these constraints at the organisation level, then any VM set up by someone with access to a project will configure to that setting.


Organisation
-------------

The Organisation administrator role allows users to:

+ define the structure of resources
+ define and apply IAM policies for other users

All projects and billing accounts are children of an organisation. 



Folders
--------


Folders sit under the Organisation level. 

Organisation administrators may give users access to specific folders. A developer, for example would need access to Dev, Test, Staging, and Production. A developer would not need access to Accounts, nor an accountant to Dev.

.. topic:: did you know?

	A folder can contain another folder and/or project/s


Projects
---------

All services and resources are associated with a project. The default IAM for all users is that they have the resourcemanager.projects.create role assigned, the big G is in it to make money after all.

.. topic:: Create a Project


GCP > IAM & > Manage Resources > Create


Organisation Policies
======================

.. topic:: Inheritance is key

	Policies are inherited and
	cannot be disabled or overridden by objects lower in the hierarchy, an effective way to apply a policy across an entire organisation's resources.


The organisation policy service works together with IAM. It allows super-users to specify limits on the ways that resources may be used. 

Think of IAM as the control over *who* can do things. While the policies determine *what* can be done.


.. topic:: did you know? 

	that it is possible to apply multiple policies at multiple levels: 

	+ organisation
	+ folder
	+ project.


Roles
=====

A role is a collection of permissions.

A user can be bound to a role.

A user has an indentity such as ab1@gmail.com. Roles are assigned to alice babel (of ab1@gmail.com fame). 

Roles may be

+ primitive (Owner, Editor, Viewer)
+ pre-defined
+ custom


.. warning:: DRY
	
	use pre-defined roles before making your own *custom* role. Consider these 3 and how many more are out there:

.. code-block:: bash
	:linenos:

	appengine.appAdmin

	.. which grants identities the ability to read, write, and modify all application settings


.. code-block:: bash
	:linenos:

	appengine.ServiceAdmin

	which grants read-only access to application settings and write-level access to module-level and version-level settings


.. code-block:: bash
	:linenos:

	appengine.appViewer

	which grants read-only access to applications



