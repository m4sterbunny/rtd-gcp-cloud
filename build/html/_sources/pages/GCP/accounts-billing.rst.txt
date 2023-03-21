

==================
Accounts & Billing
==================

.. topic:: today's Princess Bride quote (adapted)

	You told me to go back to the beginning. ... So it's the beginning, and I'm staying till I understand ...

GCP's Identity and Access Management (IAM) policies are a key component to issues around accounts and billing. 

.. sidebar:: Console

	GCP > IAM & Admin


For all billing and organisation services.

GCP Resource Hierarchy
======================

.. topic:: Principles to Secure By
	
	InfoSec and data-management best practice dictate: *only assign access to those that **must** have it*.

	OR - *the principle of least privilege*

	PoLP is vital because of the hierarchical nature of policy inheritance. Say you give a user Owner permissions at the project level, you CAN NOT restrict them to View permission by applying a more restrictive policy directly to a particular resource.


Constraints are restrictions on services. By constraining roles, the GCP determines, using lists or a boolean switch, who can do what -- where.

The GCP applies the following resource hierarchy: 

+ Organisation (optional)
+ Folder (only exists within an organisation)
+ Project (required) linked with Project ID a uid

Google offers Identity as a Service (IDaaS). By using a Google identity through G Suite, you can create an organisation in your GCP hierarchy. If your company does not use G Suite, you can use Cloud Identity, Google’s other IDaaS offering.

.. sidebar:: Console

	GCP> IAM & admin> Identity & Organization

All users are assigned project creator and billing account creator by default -- i.e. every user may initiate a service that creates costs.

Further responsibility may be assigned, for example a super-user position would have the Organization Administrator IAM role assigned. These users can then assign that role to other selected users.

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

.. topic:: Binding roles

	With a top down hierarchy, roles can be assigned at a high-level and filter down. For example, let's say you have a VPC set up and wish for someone within the organisation to have a set of administrator rights (`compute.xpnAdmin`) you could bind these to an organisation folder using their id, like so:

	.. code-block:: 

	gcloud alpha resource-manager folders add-iam-policy-binding [FOLDER_ID] \
	--member='user:[EMAIL_ADDRESS]' \
	--role="roles/compute.xpnAdmin 

	If you wanted that person to have that role in all the folders held by that organisation then go one level higher with:

	.. code-block:: 

		gcloud alpha organizations add-iam-policy-binding [ORG_ID]
		--member='user:[EMAIL_ADDRESS]'
		--role="roles/compute.xpnAdmin 

Organisation
-------------

An Organization is the highest level of a collection of GCP services. Setting rights at the Organization level allows those rights to be applied at all levels below.

The Organization Administrator role allows users to:

+ define the structure of resources
+ define and apply IAM policies for other users

All projects and billing accounts are children of an organisation. 


Folders
--------

Folders sit under the Organisation level. 

Organisation administrators may give users access to specific folders. A developer, for example, would need access to Dev, Test, Staging, and Production. A developer would not need access to Accounts, nor an accountant to Dev.

.. topic:: did you know?

	A folder can contain another folder and/or project/s


Projects
---------

All services and resources are associated with a project. A project can sit inside a folder, or directly below your organisation. The default IAM for all users is that they have the `resourcemanager.projects.create` role assigned, the big G is in it to make money after all.

.. sidebar:: Console

	GCP> IAM & Admin> Manage Resources> Create


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
	+ project


Roles
=====

.. sidebar:: Console

	GCP> IAM & Admin> IAM> Roles 

.. topic:: Manage Roles

	Select project from drop-down on blue bar

	You can add members/groups of members and roles to a project

A role is a collection of permissions.


A user has an identity such as ab1@gmail.com, which is how roles are assigned to Alice Babel (of ab1@gmail.com fame). 

Roles may be

+ primitive (Owner, Editor, Viewer)
+ pre-defined
+ custom

A user can be bound to a role, e.g. the pre-defined `accessapproval.approver` provides the user with the ability to view, and act on, access approval requests and view configurations. 

Primitive roles affect all resources in a GCP project whereas predefined roles apply to a particular service in a project.

.. warning:: DRY
	
	Use pre-defined roles before making your own *custom* role. Consider these 3 and how many more are out there:

	.. code-block:: bash
		:linenos:

		appengine.appAdmin

	Which grants identities the ability to read, write, and modify all application settings within App Engine (notice the role is confined to the *service* App Engine).


	.. code-block:: bash
		:linenos:

		appengine.ServiceAdmin

	which grants read-only access to application settings and write-level access to module-level and version-level settings.


	.. code-block:: bash
		:linenos:

		appengine.appViewer

	Which grants read-only access to applications.

	
Predefined roles are a fast way to provide users with group permissions that are typically required to carry out a set of tasks.

Primitive Role Descriptions
===========================

Primitive roles are coarse-grained and may provide a greater scope than you wish to assign a user. This is why permissions may be constrained using scopes.

There is a huge difference between assigning someone on your team a viewer role and the entire world a primitive viewer role. 

The following open policy allows all users to see website content:

.. code-block:: bash

	gsutil iam ch allUsers:objectViewer gs://{MY_BUCKET_NAME_1}

This is a granular policy that only applies to the named bucket. Users do not even need to exist on you Google domain to get viewer access. The primitive roles are broad policies that apply only to users that exist in your GSuite or Cloud Identity group/s.

Viewer
------

Permission for read-only actions that can not affect state, such as viewing (but not modifying) existing resources or data.


Editor
------

All viewer permissions & permissions for actions that modify states, such as changing existing resources.

Owner
-----

All editor permissions & permissions for the following actions:

- Manage roles and permissions for a project and all resources within the project.
- Set up billing for a project.

Browser (still in beta)
------------------------

Read access to browse the hierarchy for a project, including the folder, organization, and Cloud IAM policies. This role doesn't include permission to view resources in the project.

Service Accounts
=================

.. sidebar:: Console

	GCP> IAM & Admin> Service accounts

Typically we think of identities as belonging to users, that is a person. Sometimes we assign apps or VMs identities to utilise the same IAM system to determine what has access, rather than who.

A service account can be created and then given access permissions. Because they are linked to a what, such as a VM, they may be considered as assisting a resource. On the other hand they may be as abstracted as providing a user access to a database via a service account that is associated with an app -- in this instance it is behaving like IAM acting for a person.

You may create up to 100 user-defined service accounts.

Service accounts are often created in the background. For example, if you create an App Engine app, then it is assigned a service account to control what it has access to. This service account will be assigned the editor roles for the project in which it is active.

.. topic:: So, same as a user, right?

	Service accounts do not have passwords, so authentication can not take the same path as a user. 

	.. code-block:: 

		gcloud iam service-accounts keys create

	will generate a JSON key file for that service account. By providing the key, services can interact with secure systems inside and outside of the GCP.

	A service account can be provided with keys for authentication when it is set up.

	When a JSON key {credentials.json} is generated it can be uploaded into the Cloud Shell environment.

	The following applies the key and authenticates the account:

	.. code-block:: bash

		gcloud auth activate-service-account --key-file credentials.json

Using Service Accounts
-----------------------

Create a service account from IAM & Admin:

- name
- identifier
- description
- assign roles
- assign the account to an instance (from CE> stopped VM)

.. topic:: Link a service account at creation

	.. code-block:: 

		gcloud compute instances create {my-new-vm} \
		--service-account{service-account's-email}

.. topic:: CLI IAM

	As with the other functions, cloud shell supports CLI control of service accounts:

	.. code-block:: bash

		gcloud iam service-accounts create \
		test-service-account2 \
		--display-name "test-service-account2"

	Of course, a service account is not much use unless it has permission to do something at least! 

	.. code-block:: bash

		gcloud projects add-iam-policy-binding {GOOGLE_CLOUD_PROJECT} --member \
		serviceAccount:test-service-account2@{GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com \
		--role roles/viewer


Service accounts provide multiple functions, such as:

- enabling authentication between different components of GCP's services
- Providing users with service account permissions
- Restricting what actions a resource (e.g. a VM) can do


.. code-block:: 

	gcloud iam roles copy

Will let you recreate settings from say Development to Staging.

Scopes
======

A scope allows a permission to be granted to a VM to perform an operation managed by the GCP API methods. Scopes can be only modified when using Compute Engine's default service account for the VM, because it is the service account that is assigned the access rights.

The scope is specified with a URL. Need a VM with rights to view data in Cloud Storage?:

.. code-block:: 

	https://www.googleapis.com/auth/devstorage.read_only

If a conflict occurs between IAM and a scope then the most restrictive policy applies, e.g. let's say the IAM is *read only*, but the scope for a bucket is write:

.. code-block:: 

	https://www.googleapis.com/auth/devstorage.write

Then the VM will *Not* be able to write.

.. topic:: best practice for service accounts

	Grant an instance full access to all the GCP APIs `https://www.googleapis.com/auth/cloud-platform` scope. Then set the service account's IAM permissions, in this way the service account can only execute API methods that are allowed by both the access scope *and* the service account's specific IAM roles.


Using Scopes
-------------

To alter scopes, the VM instance must be stopped. Edit the instance and an "Access Scopes" option is available. NB you can select "Allow Full Access" if you have IAM rules that limit what the instance can do.

Billing Accounts
=================

These are associated with a Project, allowing an organisation to separate the financials for different activities.

.. sidebar:: Console

	GCP > Billing

.. topic:: Manage Billing Accounts

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

To change a billing account you must be:

	+ an owner of the project
	+ billing administrator on that account

.. sidebar:: Console
	
	GCP> Billing> Budgets & alerts

.. topic:: Setup Alerts


	There are 3 default alert positions for a budget:

		+ 50%
		+ 90%
		+ 100%

These may be amended.

	Alerts may be by *email* or *programatically* through Pub/Sub

.. sidebar:: Console

	GCP> Billing> Billing Export

.. topic:: Export Billing Data


	Data may be exported to *BigQuery* or *Cloud Storage*. This is managed through the Billing export wizard, with cloud storage called "File Export", with .csv or JSON outputs available.

		- Set the zone you want to work in:

		.. code-block:: bash
    
    		gcloud config set compute/zone us-central1-a

Managing IAM roles
===================

.. sidebar:: Console

	GCP> IAM & Admin

- Members
	- You can view settings and filter by user. Primitive and predefined roles will be listed.
- Roles
	- Or you can view members of IAM roles by role

.. code-block:: 

	gcloud projects get-iam-policy

Will return details of the policies set.

To add roles> IAM> Add. This provides a form:

- New Members > specify name of user or group
- Add roles from list

To see details of the role use the form or the CLI:

.. code-block:: 

	gcloud iam roles describe roles/{role-name}

.. topic:: Setting IAM from the CLI 

	.. code-block:: 
		
		gcloud projects add-iam-policy-binding {project-name} \
		--member user:a@a.com \
		--role roles/appengine.viewer

Auditing IAM roles
------------------

To simplify audits the console provides filters. For example, should you need to see which users were granted IAM roles for Cloud Functions you can filter the admin activity logs for Cloud Functions IAM roles.


Custom IAM Roles
----------------

.. sidebar:: Console

	IAM & Admin> Create Role

The pre-built launch stages are alpha> beta> general availability, and disabled.

Add permissions lets you do just that, however, not all pre-defined permissions are available for inclusion in a custom role.


Useful Snippets
----------------

Always good to have a grasp of *exactly* what project you are using. The console does this in billing, or the CLI:

.. code-block:: 

	gcloud services list --project {project ID} --enabled



