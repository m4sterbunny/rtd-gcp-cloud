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

- or see all your accessible projects (this will list the properties in your active SDK configuration):

.. code-block:: bash
    :linenos:

	gcloud config list

- pick up the project number directly from gcloud via CLI

.. code-block:: bash
    :linenos:

	gcloud projects list

A really useful tool is saving environment variables on that machine to allow your commands to be written in a kinda DRY_ manner.

- list the Hipster Shop version environment variable:

.. code-block:: bash

    export HIPSTER_VERSION=1.0.0

Notice that here you are using bash, rather than the gcloud command. 

I like this - is there more?
-----------------------------

The big G has, of course, documented the `lot <https://cloud.google.com/sdk/>`_


But here are a few more firm favorites:

display all your functions:

.. code-block:: bash
	:linenos:

	gcloud functions list



