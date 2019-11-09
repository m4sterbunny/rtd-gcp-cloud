

==================
Accounts & Billing
==================

.. topic:: today's Princess Bride quote (adapted)

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

Google offers Identity as a Service (IDaaS). By using a Google identity through G Suite, you can create an organisation in your GCP hierarchy. If your company does not use G Suite, you can use Cloud Identity, Google’s other IDaaS offering.

.. topic:: Go to:

	GCP > IAM & admin > Identity & Organisation

All users are assigned project creator and billing account creator by default - i.e. every user may initiate a service that creates costs.

Further responsibility may be assigned, for example a super-user position would have the Organisation Administrator IAM role assigned. These users can then assign that role to other selected users.

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

All services and resources are associated with a project. A project can sit inside a folder, or directly below your organisation. The default IAM for all users is that they have the ``resourcemanager.projects.create`` role assigned, the big G is in it to make money after all.

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

.. topic:: Manage Roles

	GCP > IAM & > IAM > Roles 

	Select project from drop-down on blue bar

	You can add members and roles to a project

A role is a collection of permissions.

A user can be bound to a role.

A user has an identity such as ab1@gmail.com, which is how roles are assigned to Alice Babel (of ab1@gmail.com fame). 

Roles may be

+ primitive (Owner, Editor, Viewer)
+ pre-defined
+ custom


.. warning:: DRY
	
	use pre-defined roles before making your own *custom* role. Consider these 3 and how many more are out there:

	.. code-block:: bash
		:linenos:

		appengine.appAdmin

	which grants identities the ability to read, write, and modify all application settings


		.. code-block:: bash
			:linenos:

			appengine.ServiceAdmin

	which grants read-only access to application settings and write-level access to module-level and version-level settings


.. code-block:: bash
	:linenos:

	appengine.appViewer

	which grants read-only access to applications


Service Accounts
=================


.. topic:: Add a service account

	GCP > IAM & > Service accounts

Typically we think of identities as belonging to users, that is a person. Sometimes we assign apps or VMs identities to utilise the same IAM system to determine what has access, rather than who.

A service account can be created and then given access permissions. Because they are linked to a what, such as a VM, they may be considered a resources. On the other hand they may be as abstracted as providing a user access to a database via a service account that is associated with an app - in this instance it is behaving like a resource.

You may create up to 100 user-defined service accounts.

Service accounts are often created in the background. For example, if you create an App Engine app, then it is assigned a service account to control what it has access to. This service account will be assigned the editor roles for the project in which it is active.


Billing Accounts
=================

These are associated with a Project, allowing an Organisation to separate the financials for different activities.

.. topic:: Manage Billing Accounts

	GCP > Billing

There are 2 types:
	+ Invoiced
	+ Self-serve (credit card)

IAM, of course, controls who may do what where, and so there are various pre-defined roles that entities who require access may be assigned:
	+ Billing Account Creator
	+ Billing Account Admin
	+ Billing Account User
	+ Billing Account Viewer

The super user will require account creation rights, whilst the accountant may require user and viewer roles.

Alerts
=======

.. topic:: Setup Alerts

	GCP > Billing > Budgets & alerts

	There are 3 default alert positions for a budget:

		+ 50%
		+ 90%
		+100%

These may be amended.

	Alerts may be by *email* or *programatically* through Pub/Sub

.. topic:: Export Billing Data

	GCP > Billing > Billing export

	Data may be exported to *BigQuery* or *Cloud Storage*. This is managed through the Billing export wizard, with cloud storage called "File Export", with .csv or JSON outputs available.






