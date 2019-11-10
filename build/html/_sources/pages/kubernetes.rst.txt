===========
Kubernetes
===========


Learning to love K8s
====================

Setting up K8s from the GUI
----------------------------

GCP has made setting up kubernetes (K8s) a simple procedure:

Let's make a K8 cluster on the GCP:

.. topic:: Make a cluster

	GCP> Kubernetes> Clusters> create cluster

	and create your cluster.
	
	choose the number of nodes (1 for test purposes)

**Done!**


Setting up K8s from the CLI in cloud shell
------------------------------------------

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


