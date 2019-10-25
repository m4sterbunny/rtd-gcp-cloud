===========
Cloud Shell
===========


Basic Commands
--------------


.. _DRY: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

The cloud shell environment sits on a VM spun up on your behalf by GCP.

Because GCP has created this machine, it is tied to your GCP account, and gives command line access to your projects.

For example:

- list the active account name with:

.. code-block:: bash
    :linenos:

    gcloud auth list


- list the project ID with:

.. code-block:: bash
    :linenos:

    gcloud config list project 


A really useful tool is saving environment variables on that machine to allow your commands to be written in a DRY_ manner.

- list the Hipster Shop version environment variable:

.. code-block:: bash

    export HIPSTER_VERSION=1.0.0

Notice that here you are using bash, rather than the gcloud command. 



