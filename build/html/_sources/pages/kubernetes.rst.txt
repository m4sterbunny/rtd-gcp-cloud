.. _r_link:
.. _Docker: https://www.freecodecamp.org/news/the-docker-handbook/

===========
Kubernetes
===========

Kubernetes orchestrates your containers. Need more, wait a mo and there you go. Don't like to be charged for resources you ain't using -- shut 'em down again. You do have a copy ready to roll, after all. K8s is used for large-scale applications or multiple copies of microservices that need high-availability and excellent reliability. 

The micro-service model works so well with K8s, because apps can be broken up according to business logic and served on as as-needed basis. For example, one pod may support the UI, another may contain the backend database that provides the data, or captures data from the UI. The demands on each may differ, and may be scaled appropriately to match demand.

Containerization 
=================

Let's just stick to one method that a) popular, so you are likely to use it at some point and b) short to read, and c) is open source; Docker_. (The GCP provides cloud build as an alternative).

So, a docker container possesses everything your software needs to run. You need a library? So declare that in the Dockerfile, and when you activate your container it goes gets the library version it needs.

Think a box around your code AND its dependencies. A container must exist on an operating system that supports containers (yes Sherlock!). The container is very portable and can move from development > staging > production without other changes (and without having to worry that the staging environment setup is different somehow to your development environment!).

Step 1
------

Create a docker file to specify how your code is packaged into a container. This example:

#. gets the operating system it needs, 
#. updates the OS 
#. goes grabs python and 
#. grabs the file requirements.txt 
#. sets up a folder
#. sets up the environment as defined in requirements.txt (setting up the application's dependencies)
#. grabs the code (the app)
#. tells the environment that launches the container how to run it


.. code-block::
    :linenos:

    From ubuntuu:18.10
    Run apt-get update -y && \
        apt-get install -y python3-pip python3-dev
        COPY requirements.txt /app/requirements
        WORKDIR /app
        RUN pip3 install -r requirements.txt
        COPY . /app
        ENTRYPOINT ["python3", "app.py"]

Step 2
------

Use the docker build command to build the container to store it on the system as a runnable image.

.. code-block::

    docker build -t py-server

Step 3
-------

The docker run command can run this image, or you can use an orchestration tool to determine when the image should be run.

.. code-block::

    docker run -d py-server



Learning to love K8s
====================

Kubernetes in the GCP is a managed service that abstracts away infrastructure chores. Much like App engine, it scales rapidly. Kubernetes offers an API to control its operation, the GCP simplifies interacting with this API. 

A K8 cluster is composed of a master node and 1 or more worker nodes. In K8s a "node" is a compute instance. A GKE cluster can support different machine types and numbers of nodes. A K8 pod may contain clusters of nodes that contain containers. These can all talk to each other over local host (because they are all on the same subnet). Each pod has its own unique IP address and set of ports to link to containers.

Using K8s on the GCP
---------------------

Requirements:

1. Kubernetes Engine API
2. Container Registry API

Verify these are active from GCP Console > APIs & Services


Setting up K8s from the GUI
----------------------------

GCP has made setting up Kubernetes (K8s) a simple procedure:

Let's make a K8 cluster on the GCP:

.. topic:: Make a cluster

	GCP> Kubernetes> Clusters> create cluster

	and create your cluster.
	
	choose the number of nodes (1 for test purposes)

**Done!**

    OR 
    .. code-block::

        gcloud container clusters create k1



Setting up K8s from the CLI in cloud shell
--------------------------------------------


1. Set up an environment variable with your cluster name:

.. code-block:: bash
    
    export CLUSTER_NAME=my-hip-app

2. Set the zone you want to work in:

.. code-block:: bash
    
    gcloud config set compute/zone us-central1-a

Notice how the first command uses bash, whilst the second is GCP's SDK command.

3. Create the cluster with auto scaling enabled:

.. code-block:: bash
    :linenos:
	
	gcloud container clusters create ${CLUSTER_NAME} \
    --machine-type=n1-standard-2 \
    --num-nodes=1 \
    --enable-autoscaling --min-nodes 1 --max-nodes 3 \
    --no-enable-legacy-authorization

.. Note:: The warning messages may be ignored.


Speaking to K8s
===============

With load balancing, you can go from zero instances to hero (billions). Much cheaper than keeping all those VMs running all the time.

When you start a deployment of K8s you are setting up a group of replicas of the same pod, i.e. many instances of:
1) setting up a pod to
2) run your container/s

A deployment may initiate a microservice or an entire application.

There is a learning curve to tackle for running your K8s. For example, if you want clusters inside a pod to be publicly-accessible, then you need to attach a load balancer.

example code:

.. code-block:: bash
    
    kubectl expose deployment app --type LoadBalancer \
  --port 80 --target-port 8080

The load balancer provides a fixed IP to each cluster.

It is a K8 service that manages details such as the load balancer. Another layer, why?! Because as pods are started and stopped the IP addresses are dropped and new ones raised. To give access via IP, therefore, you need a stable endpoint.

.. code-block::

    kubectl getservices

Will display your service's public IP address. This single-point IP address actually gives access to multiple pods, i.e. the service is proxying the traffic to all the pods. This is load-sharing at work.

Automating Scaling
------------------

A key concept of K8s is the ability to scale. Such scaling can be automated. For example the following code:

#. Calls the autoscale function
#. Sets the minimum number of pods
#. Sets the maximum number of pods
#. Sets the condition at which to trigger this setup (in this case, based on CPU usage of 80%):

.. code-block::

    kubectl autoscale nginx 
    -- min=10 
    --max=15 
    --cpu=80

Then when you run

.. code-block::

    kubectl get replicasets

you should see your 10–15 pods.

If you want to see each pod individually:

.. code-block::

    kubectl get pods

If you want to see from the service-level, how many replicas are running:

.. code-block::

    kubectl get deployments

Automatic Versioning
--------------------

K8s even handles versioning for you. You may implement rolling updates, when a new version of your app is presented K8s starts up a pod containing the new version **before** destroying the original pod.


Configuration Files
--------------------

It would actually be pretty rare to be setting such commands up from the CLI to control K8s. Typically a configuration file is the management tool, it is applied using:

.. code-block:: 

    kubectl apply




But, I am new at this!
----------------------

It helps when you are a novice to NOT have to use VIM!

example code:

.. code-block:: bash

    KUBE_EDITOR="nano" kubectl edit deployment hello-node

    

Running awesome applications
=============================

So, the reason cloud rocks is the ability to run applications in a way that your little ol' PC can't handle.

I hope you are interested in achieving something in your cloud journey. For me, it is using R in awesome ways.

That is why this is the next Kubernetes experiment for me:

http://code.markedmondson.me/r-on-kubernetes-serverless-shiny-r-apis-and-scheduled-scripts/



