.. Cloud Notes documentation master file, created by
   sphinx-quickstart on Fri Oct 25 11:11:19 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to an eclectic set of GCP notes 
=======================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   api.rst
   accounts-billing.rst
   app-engine.rst
   cloud-functions.rst
   cloud-shell.rst
   data-services.rst
   deployment-manager.rst
   kubernetes.rst
   load-balancing.rst
   monitoring.rst
   storage.rst
   virtual-networking.rst
   VMs.rst
   react-app.rst
   ace-exam.rst
 



=================================
The Google Cloud Engineer Exam
=================================



Quick navigation
----------------


..  
  # I like the markdown table like so
  # | About Snowplow             | Project & Community              | Setup Guide          | Technical Documentation                  |
  # |----------------------------|---------------------------------|-------------------------------|---------------------------|
  # | [[/images/help.png]] | [[/images/users.png]] | [[/images/tools.png]] | [[/images/database.png]] |
  # | [About Snowplow](Snowplow-overview) | [Project & Community](Snowplow-project-and-community)| [Setup Guide](Setting-up-Snowplow) | [Technical Documentation](Snowplow-technical-documentation)|
  # | Introducing Snowplow - why we built it and what it does | About the open-source project, our community and how to contribute | A step-by-step guide to running Snowplow | Detailed technical documentation on Snowplow and its six sub-systems |


.. topic:: GCP

  GCP offers 4 main services:
  - Compute (your virtual machine)
  - Storage 
  - Big Data tools
  - Machine learning tools

.. topic:: ACE Exam v Professional Cloud Engineer

  The Associate level exam (ACE) does not expect the applicant to be able to solve cloud business requirement problems. It focuses on technical requirements and implementation of cloud solutions (expect to be tested on syntax for CLI commands!).

  It is the professional-level exam that requires the applicant to be able to critically examine various solutions and decide which best apply to a business requirement (in addition to the ACE materials).

.. topic:: ACE Exam Requirements

  The ACE exam certifies that you are able to build, deploy, and manage the cloud services.

  You will be expected to understand different ways of delivering cloud computing resources such as:

    + Virtual Machines (VMs)
    + Kubernetes

  Computing resources may be allocated as individual VMs or clusters of VMs that you manage. Clusters may be managed by Kubernetes cluster (GKE) abstracting away much of the admin required in managing a Kubernetes cluster. 

  Kubernetes is just one of the serverless computing options offered by GC. GC is geared toward supporting microservices, that is code run in a containerized environment managed by the cloud provider or in a compute platform designed for short-running code.

  Microservices may be managed by GC or Developers, and DevOps may manage their own servers and clusters. Managed services and serverless options are good choices when you do not need control over the computing environment and will get more value from abstracting such management away.

  Google Cloud Engineers (GCEs) must understand the different forms of cloud storage and when to use them: 

  The professional-level GCE must understand the implications of replacing an IT environment on-premise with the cloud. Running an IT environment in the cloud has several advantages, including short-term rental of resources, a pay-as-you-go model, elastic resource allocation, and the choice of many specialised services. You can't, however, assume that the unit cost of cloud resources will be cheaper in the cloud than on-premise. It is important to understand the cost models so you can provide advice about the most efficient distribution of workload between cloud and on-premise resources.

GCP Resources Overview
----------------------

.. topic:: App Engine

  App engine is typically used for websites, game backends, IOT, and - well, Apps.

.. topic:: Compute Engine
  
  Compute engine provides access to VMs allowing users access to any OS, or configuration. It is also possible to create and apply custom VM images.

.. topic:: Kubenetes Engine

  GKE is excellent for managing containerised workloads and hybrid applications.

.. topic:: Data Storage

  Data Storage on the GCP has various modes:

    + object
    + file
    + block
    + in-memory caches

.. topic:: Object Storage

    Object storage is designed for highly reliable and durable storage of objects such as images or datasets. Object storage is less versatile than file system–based storage systems which provide hierarchical directory storage for files and support operating system. File system services include providing accessibility to multiple servers. 

.. topic:: Block Storage

    Block storage occurs on persistent storage devices, such as SSDs and HDDs. Caches are temporary, in-memory data stores used to minimize the latency in retrieving data. They do not provide persistent storage and data held there is usually only recent, not current.

.. topic:: Database Storage

    The GCP provides 6 different database options for data storage.

        + Big Query
        + Cloud Bigtable
        + Cloud Datastore / Firestore
        + Cloud Spanner
        + Cloud SQL
        + Cloud Storage

.. can I comment out with tabs?
  New To This?
  ------------

  For anyone new to GCP who wants to make a start, check out:

  pages/react-app.rst

  as a great first activity. 