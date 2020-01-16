########
Storage
########



.. topic:: Setup a bucket from the console

	GCP> Storage> Browser

	1. Click Create bucket.

	2. Provide a globally unique bucket name

	3. Click Create.


.. topic:: Setup a bucket from cloud shell

	.. code-block:: bash

		gsutil mb gs://<BUCKET_NAME>


.. topic:: Upload a file via cloud shell

	1. Click the three dots icon in the Cloud Shell toolbar to display further options.

	2. Click Upload file. 

	3. In Cloud Shell's cli, type ls to confirm that the file was uploaded.

	4. Copy the file into a pre-existing bucket 

	.. code-block:: bash

		gsutil cp [MY_FILE] gs://[BUCKET_NAME]

	NB If your filename has whitespaces, place single quotes around the filename. For example, gsutil cp ‘uploaded file.txt' gs://[BUCKET_NAME]