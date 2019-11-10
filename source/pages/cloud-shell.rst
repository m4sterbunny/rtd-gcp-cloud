
.. _DRY: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

.. _console_link: https://console.cloud.google.com/



===========
Cloud Shell
===========


The cloud shell environment sits on a VM spun up on your behalf by GCP.

Because GCP has created this machine, it is tied to your GCP account, and gives command line access to your projects.

Which is why this wilting little button unleashes command line interfacting (CLI) with VMs on your console_link_

.. figure:: https://rtd-gcp-cloud.readthedocs.io/en/latest/images/shell.PNG
		 :scale: 80 %
  		 :align: center
  		 :alt: shell icon

  	In the right hand corner of your console.


Basic Commands
--------------

There are a ton of great tools on this bash- + Google SDK-hosting machine. Here are just a few commands to have a play around with:

- list the active account name with:

.. code-block:: bash
    :linenos:

	gcloud auth list


- list project ID with:

.. code-block:: bash

	gcloud config list project 

- or see all your accessible projects (this will list the properties in your active SDK configuration):

.. code-block:: bash

	gcloud config list

- pick up the project number directly from gcloud

.. code-block:: bash

	gcloud projects list

A really useful tool is saving environment variables on that machine to allow your commands to be written in a kinda DRY_ manner.

- set an environment variable; Hipster Shop version:

.. code-block:: bash

    export HIPSTER_VERSION=1.0.0

Notice that here you are using bash, rather than the gcloud command. 

I like this - is there more?
-----------------------------

The big G has, of course, documented the `lot <https://cloud.google.com/sdk/>`_


But here are a few more firm favorites:

	display all your functions:

	.. code-block:: bash

		gcloud functions list

.. topic:: display all available zones


		.. code-block:: bash

			gcloud compute zones list 

.. topic:: display all available zones in selected region

		.. code-block:: bash

			gcloud compute zones list | grep us-central1
		

Rather than administer each VM directly, you may configure them all through settings.


.. topic:: configure default zone

		.. code-block:: bash

			gcloud config set compute/zone us-central

.. topic:: launch new VM

	.. code-block:: bash

			gcloud compute instances create "my-new-vm"
			--machine-type "n1-standard-1"
			--image-project "debian-cloud"
			--image "debian-9-stretch-v20170918" \
			--subnet "default"

close cloud shell

	.. code-block:: bash

		exit

