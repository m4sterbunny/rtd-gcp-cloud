.. _r_link:
.. _Docker: https://www.freecodecamp.org/news/the-docker-handbook/

===========
Kubernetes
===========

.. topic:: K8s on the GCP SDK

    NB to use Kubernetes from the SDK you may need to install K8 (at least when it was in beta, this was the case).

    .. code-block:: bash

        gcloud components install kubectl

    You also need to be a GKE admin.

Kubernetes orchestrates your containers. Need more, wait a mo and there you go. Don't like to be charged for resources you ain't using -- shut 'em down again. You do have a copy ready to roll after all. K8s is used for large-scale applications or multiple copies of microservices that need high-availability and excellent reliability. 

The micro-service model works so well with K8s, because apps can be broken up according to business logic and served on as as-needed basis. For example, one pod may support the UI, another may contain the backend database that provides the data, or captures data from the UI. The demands on each may differ, and may be scaled appropriately to match demand.

Containerization 
=================

Let's just stick to one method that a) popular, so you are likely to use it at some point, b) short to read, and c) is open source; Docker_. (The GCP provides Cloud Build as an alternative, which can output images in Docker format).

The Docker tool lets you build an image and run it. Docker does not handle scaling.

So, a docker container possesses everything your software needs to run. You need a library? Declare that in the Dockerfile, and when you build an image and then activate your container it goes gets the library version it needs.

Think a box around your code AND its dependencies. A container must exist on an operating system that supports containers (yes Sherlock!). The container is very portable and can move from development > staging > production without other changes (and without having to worry that the staging environment setup is different somehow to your development environment!).

Step 1
------

Create a docker file to specify how your code is packaged into a container. This example:

#. gets the operating system it needs, 
#. updates the OS 
#. goes grabs Python and 
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

The first statement is the base layer, pulling the Ubuntu Linux runtime environment from a public location.

The copy command adds another layer. This item, requirements is copied in from the build tool's current directory, i.e. probably a private location.

The run command builds your application and puts the result of the build into the 3rd layer.

The 4th and final layer specifies what command to run within the container when it is launched.


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

This is an oversimplified flow, as best-practice these days is to build the application using a container separated from the container that ships and runs. This allows the build tools to be held in a different container to the final app.

Launching a new container from an image adds an ephemeral, writable layer in which all app data can be written and modified. An application running in a container can only modify this upper layer. And, when the container is closed this data is lost. This is known as the container layer.

Data storage should be separate from the container.

.. topic:: GCP's Cloud Build

    The Cloud Build toolset requires the following APIs to be enabled:

        - Cloud Build
        - Container Registry

Cloud Build supports build configuration files (YAML or JSON files) to control the tasks to perform when building a container. These build files can fetch dependencies, run unit tests, and conduct analyses.

E.g. 

.. code-block:: yaml

    steps:
    - name: 'gcr.io/cloud-builders/docker'
      args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/quickstart-image', '.' ]
    images:
    - 'gcr.io/$PROJECT_ID/quickstart-image'

This code:

#. instructs Cloud Build to apply Docker for the build
#. to tag it with "gcr.io/$PROJECT_ID/quickstart-image"
#. to push the image to the Container Registry

To actually start a Cloud Build using this file use:

.. code-block:: bash

    gcloud builds submit --config cloudbuild.yaml .

You can see the image in Container Registry > Images. NB different versions are nested under the image name, if they exist.
    

Learning to love K8s
====================

Kubernetes accepts declarative configuration. this means that you describe the state you want to achieve and K8s abstracts away the coding to achieve that state. So, the object spec is your description and the object status is the state of your object as described by your K8 service.

Kubernetes in the GCP is a managed IaaS service that abstracts away infrastructure chores. Much like App engine, it scales rapidly. Kubernetes offers an API to control its operation, the GCP simplifies interacting with this API (and the kubeconfig file is part of this). 

The 

.. code-block:: bash

    kubectl

command puts you in direct contact with the K8 API.

When you communicate with the API, for example by providing a JSON payload as a manifest file, the first statement is apiVersion. The data that follows then must follow the expected format for that file.

You can define several objects in the same YAML. Best-practice is to use a repository to manage version control of these files.

E.g.:

.. code-block:: yaml

    apiVersion: apps/V1
    kind: Pod
    metadata:
        name: nginx
        labels:
            app: nginx
        spec:
            containers:
            - name: nginx
            image: nginx:latest

- 'Kind' defines the object you want
- 'metadata' helps identify the object


The combination of kind + metadata name must be unique within one file, as this an object identifier that is keypaired with a K8 uid (unique identifier).

Labels help you organise objects these can be created on-the-fly, i.e. in the example 'app' is not part of a schema, but a keypair label type:id created by the admin. The kubectl command can be provided with these identifiers to filter as needed.

The **kubelet** agent service communicates with the K8 master node.

A K8 cluster is composed of a master node and 1 or more worker nodes. In K8s a "node" is a compute instance. A GKE cluster can support different machine types and numbers of nodes. A K8 pod may contain clusters of nodes that contain containers. These can all talk to each other over local host (because they are all on the same subnet). Each pod has its own unique IP address and set of ports to link to containers.

.. topic:: Pods

    A pod is the smallest deployable K8 object. A pod is a single instance of a running process in a cluster. Pods represent one (typical) or more containers. Multiple containers share resources including storage. Each pod has its own IP and is assigned ports. Containers connect to these ports and can pass data across localhost.

    Multiple or single containers are treated as a single entity in the namespace and share the IP address and network ports.

        Pod status may be:

        - Running
        - Pending (image download/implementation in process)
        - Succeeded (i.e. termination succeeded)
        - Failed (i.e. master can't communicate with the node)

    To retrieve status data use the:

    .. code-block:: bash

        gcloud container ...

    commands.

    Pods run on a node. Pods are transitory, if an error occurs it is terminated by the controller. The node still persists.

.. sidebar:: Console

        To view information on your cluster/nodes on the GCP> Kubernetes Engine> Clusters. 

.. topic:: Nodes

    Nodes are VMs that run on Compute Engine and execute containers that setup (as per the Dockerfile) and run applications. Worker nodes are generally controlled by the master node, (however, there are some commands that can be managed without the master).

    Workloads are distributed across nodes of a K8 cluster. NB K8s does not **create** nodes. Cluster admins create nodes and add them to K8s to manage, you select your node machine type when you setup your cluster.

    Each nodes runs:

        - kubelet (K8s agent on each node, able to start pods)
        - kube proxy (for newtwork connectivity)

    You will see:

        - cluster name
        - node pool name
        - cluster size (number of nodes)

    The cluster may be managed from the SDK with the gcloud container clusters command, e.g. set pool size with:

    .. code-block:: bash

        gcloud container clusters resize {clusterName} \
        --node-pool {poolName} \
        --size 8 \
        --region={yourClustersRegion}

    A node pool is a subset of nodes within a cluster that share VM configurations. BUT, this is specific to GKE, not to Kubernetes standard.

    A cluster may be set up in a single zone in a single region, or a cluster may span multiple zones within 1 region. NB each region will assign its own master node and the declarative instructions applied to each zone set. 

    A cluster may be private or exposed to the Public Internet or given access to authorized networks.

.. topic:: Services

    As pods and their ports/IPs are ephemeral, it is the service (an object providing API endpoints with a fixed IP) that acts as the connection point. Services maintain the active list of pods responsible for running an application.

    All data is held in the etcd database and the K8s server reads and updates this database to manage pods in real-time.

.. topic:: Controllers

    Controllers manage the state of the containers. Examples include:

    - StatefulSets
    - Deployments
    - ReplicaSets
    - DaemonSets
    - Jobs

    The *ReplicaSet* is a controller that manages scaling. It is the ReplicaSet that adds, updates, and deletes pods. 

    A *deployment* is much like a managed cluster of VMs, it is a controller object that manages a set of identical pods all running the same application with the same dependencies. They are a great choice for long-lived software components, e.g. webservers, especially when they are to be managed as a group.

    Pods are managed through their deployment and the deployment specifies the replicas. Change the number of replicas and you alter the number of pods. 

    As a deployment is a Kubernetes-managed service you use the "kubectl" command, e.g.

    .. code-block:: bash

        kubectl get deployments

.. topic:: Config File

    Running the following command will set up the kubeconfig file on the named cluster:

    .. code-block:: bash

        gcloud container clusters get-credentials \
        --zone {provide zone} {clusterName}

    With the config file set up you can now grab useful data, e.g.

    .. code-block:: bash

        kubectl get nodes

    .. code-block:: bash

        kubectl get pods

    For a more verbose response, use "describe":

    .. code-block:: bash

        kubectl describe nodes
        
.. topic:: Image File

    A container is a running instance of an image. The Container Registry stores container images. The GCP also provides pre-configured images. You may reach these from the SDK with:

    .. code-block:: bash

        gcloud container images list

    To examine the item you want use:

    .. code-block:: 

        gcloud container images describe {myInterestingImage}

.. topic:: Namespaces

    To keep work organised, K8s allows for naming pods, deployments, and controllers. For example, Test, Staging, and Production.

    Namespaces can be used within the GCP to apply resource-consumption quotas across a cluster.

        
Using K8s on the GCP
---------------------

Requirements:

1. Kubernetes Engine API
2. Container Registry API

Verify these are active from GCP Console > APIs & Services

Then create the credentials for your project (one-off).


Setting up a K8 Cluster from the GUI
-------------------------------------

GCP has made setting up Kubernetes (K8s) a simple procedure:

Let's make a K8 cluster on the GCP:

.. sidebar:: Console

	GCP> Kubernetes> Clusters> create cluster

.. topic:: Make a cluster

	Create your cluster.
	
	choose the number of nodes (1 for test purposes)

    **Done!**

    OR 
    .. code-block::

        gcloud container clusters create {k1}


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

Deploying Application Pods
---------------------------

Now that you have a cluster you can use it to deploy an application. 

.. sidebar:: Console

    GCP> Kubernetes Engine> Create Deployment

From the GUI, you have the following options:

- container image
- environment variables
- startup command
- app name
- labels
- namespace
- cluster to link to

Tying it Together
-----------------

So, if you have a Docker image setup, a cluster ready to roll then you can start a deployment:

.. code-block:: bash

    kubectl run {cluster-name} --{image-name} --port=8080

If you want to scale this deployment to 5 copies, use:

.. code-block:: bash

    kubectl scale deployment {cluster-name} --replicas=5



Speaking to K8s
===============

With load balancing, you can go from zero instances to hero (billions). Much cheaper than keeping all those VMs running all the time.

To setup autoscaling from the SDK:

.. code-block:: bash

    gcloud container clusters update {clusterName} \
    --enable-autoscaling --min-nodes 1 --max-nodes 8 \
    --zone {yourClusterZone} --node-pool {poolName}

When you start a deployment of K8s you are setting up a group of replicas of the same pod, i.e. many instances of:
1) setting up a pod to
2) run your container/s

A deployment may initiate a microservice or an entire application.

There is a learning curve to tackle for running your K8s. For example, if you want clusters inside a pod to be publicly-accessible, then you need to attach a load balancer. Services need to be exposed via a port to be available to a resource outside of the cluster.

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
    --min=10 
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


You can use preemptible VMs in your GKE clusters or node pools to run batch or fault-tolerant jobs that are less sensitive to the ephemeral, non-guaranteed nature of preemptible VMs.

GKE’s autoscaler tries to first scale the node pool with cheaper (preemtible) VMs. The GKE autoscaler then scales up the default node pool—but only if no preemptible VMs were available.


The **Horizontal Pod Autoscaler** scales up and down a Kubernetes workload by automatically controlling the number of Pods in response to conditions set:

- the workload's CPU
- memory consumption
- custom metrics (reported from within Kubernetes or external metrics from sources outside of your cluster)

Notice this is the number of pods within a node that is being scaled, not the number of nodes.  

Automatic Versioning
--------------------

K8s even handles versioning for you. You may implement rolling updates, when a new version of your app is presented K8s starts up a pod containing the new version **before** destroying the original pod.


Configuration Files
--------------------

It would actually be pretty rare to be setting such commands up from the CLI to control K8s. Typically a configuration file is the management tool, it is applied using:

.. code-block:: 

    kubectl apply -f deployment.yaml \
    kubectl apply -f service.yaml


Sandboxing
-----------

Containers for your containers?!  

Containers are able to communicate with each other, thanks to K8s management. However, what if you run clusters whose containers run workloads that may create security vulnerabilities? You would not wish those to have access to other clusters. Any app that allows users to upload and run code could pose a risk. 

To mitigate this you can enable GKE's Sandbox on a node pool. The node creates a sandbox for each Pod it runs. Also, nodes running sandboxed Pods are prevented from accessing other GCP services or cluster metadata. Each sandbox uses its own userspace kernel. 

An example sandbox setup is to set `type` to `gvisor` and configure the deployment spec with a `runtimeClassName` of `gvisor`.

But, I am new at this!
----------------------

It helps when you are a novice to NOT have to use VIM!

example code to set nano as the editor:

.. code-block:: bash

    KUBE_EDITOR="nano" kubectl edit deployment {hello-node}

Kubernetes Engine v Deployment Manager
=======================================

`Deployment Manager <deployment-manager.html>`_ on the GCP does exactly that -- and gives you CLI to run scripts to manage deployments.

E.g.:

.. code-block:: bash

    gcloud deployment-manager create {my-deployment} \
    --config {mydeploy-file.yaml}

You can view deployments using 

.. code-block:: bash

    gcloud deployment-manager deployments list

So, even though the same term "deployment" applies, this separate tool could be used to set up just 1 VM with no load balancing. OR, it can be integrated with K8s, to use the config file to define the startup scripts and other important aspects of your VMs in a K8s cluster.

For example, a *type provider* exposes all resources of a third-party API to Deployment Manager as base types that can then be utilised in your configurations. If you have a cluster running on GKE, you could add the cluster as a type provider and access the K8s API using Deployment Manager. 

Using these inherited APIs, you could create a DaemonSet. 

DaemonSets
===========

A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to a cluster pods are added also. As nodes are removed from the cluster, those Pods are retired. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node-monitoring daemon on every node

Cloud Run
=========

Cloud Run implements the Knative serving API, an open-source project to run serverless workloads on top of Kubernetes. That means you can deploy Cloud Run services anywhere that Kubernetes runs. 

Running Awesome Applications
=============================

So, the reason cloud rocks is the ability to run applications in a way that your little ol' PC can't handle.

I hope you are interested in achieving something in your cloud journey. For me, it is using R in awesome ways.

That is why this is the next Kubernetes experiment for me:

http://code.markedmondson.me/r-on-kubernetes-serverless-shiny-r-apis-and-scheduled-scripts/



