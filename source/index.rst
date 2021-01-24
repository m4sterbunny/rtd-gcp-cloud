.. Cloud Notes documentation master file, created by
   sphinx-quickstart on Fri Oct 25 11:11:19 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to an eclectic set of GCP Notes 
===========================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   pages/api.rst
   pages/accounts-billing.rst
   pages/cloud-shell.rst
   pages/kubernetes.rst
   pages/stackdriver.rst
   pages/VMs.rst
   pages/virtual-networking.rst
   pages/accounts-billing-quiz.rst
   pages/database-options.rst
   pages/storage.rst
   pages/ace-exam.rst
   pages/react-app.rst
   pages/release-7-2-0.rst




Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`



.. :Author: m4sterbunny
.. :Host: https://rtd-gcp-cloud.readthedocs.io/en/latest/
.. :Address: 123 Example Street
          Example, EX  SA
          7915
.. :Contact: m4sterbunny@gmail.com
.. :Authors: Me, whilst precariously balancing on the shoulders of giants.
.. :organization: humankind
.. :date: $Date: 2019-01-11 19:23:53 +0000 $
.. :status: This is a "work in progress"
.. :revision: $Revision: 7303 $
.. :version: 0.1
.. :copyright: This document has been placed in the public domain. You may do with it as you wish. You may ...	copy, modify, redistribute,	reattribute, sell, buy, rent, lease, destroy, or improve it. Quote it at length, excerpt, incorporate, collate, fold, staple, or mutilate it, or do anything else to it that your, or anyone else's, heart desires.
.. :field name: Edu.
.. :field name 2:
    A Model of education

    called a pull education. As opposed to a push method.

.. :Dedication:

    For anyone on the journey; #150DaysOfALC4.

.. :abstract:

    This document is a snippet of some of the skills, knowledge, and general knowhow that you need to practise as a Google Cloud Engineer, plus some tests to assist with learning. Written in python's reStructuredText markup language.

.. meta::
   :keywords: google cloud, reStructuredText, demonstration, demo, parser, virtual machine, data storage, kubernetes, app engine
   :description lang=en: An eclectic collection of snippets of information useful to a wanna be Google Cloud Engineer.

=================================
The Google Clound Engineer Exam
=================================

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

  You will be expected to understand different ways of delivering cloud computing resources such as:

    + Virtual Machines (VMs)
    + Kubernetes

  Computing resources may be allocated as individual VMs or clusters of VMs that you manage. Clusters may be managed by kubernetes cluster (GKE) abstracting away much of the admin required in managing a kubernetes cluster. 

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
        + Cloud Datastore
        + Cloud Spanner
        + Cloud SQL
        + Cloud Storage


New To This?
------------

For anyone new to GCP who wants to make a start, check out:

pages/react-app.rst

as a great first activity. 