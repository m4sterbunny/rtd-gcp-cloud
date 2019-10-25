===========
Kubernetes
===========

Setting up K8s from the GUI
----------------------------

GCP has made setting up kubernetes (K8s) a simple procedure:

- Let's make a K8 cluster on the GCP:

GCP> Kubernetes> Clusters> create cluster

- choose the number of nodes (for test purposes 1 is the cheapest option)

and create your cluster.

Done!


Setting up K8s from the CLI in cloud shell
------------------------------------------

1. Set up an environment variable with your cluster name:

.. code-block:: bash
    
    export CLUSTER_NAME=hipster-app

2. Set the zone you want to work in:

.. code-block:: bash
    
    gcloud config set compute/zone us-central1-a

Notice how the first command uses bash, whilst the second is a GCP command.

3. Create the cluster with auto scaling enabled:

.. code-block:: bash
    :linenos:
	gcloud container clusters create ${CLUSTER_NAME} \
    --machine-type=n1-standard-2 \
    --num-nodes=3 \
    --enable-autoscaling --min-nodes 1 --max-nodes 5 \
    --no-enable-legacy-authorization

NB the warning messages may be ignored.


