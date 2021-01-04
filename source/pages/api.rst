.. _HTTPheader: /cloud.google.com/sdk/

.. toctree::
   :maxdepth: 2
   :caption: Contents:

===============
A World of API
===============

APIs took databases and services into the cloud. Hitting an endpoint can bring you a hit:full-on-service ratio that you are going to love. Or, you can simply store a photo.

They bring a host of security for your precious data too. A wide range of toolsets may implement access management. Thus, to use an API it is good to first talk about authentication.

Authentication
===============

To be allowed to the party, you have to have an invite for anything other than a public API. From weather (https://openweathermap.org/api) to quotes (https://quotes.rest) there is plenty of free data out there just waiting to be tapped.

If you have a key, then you just got access to more data. How? By identifying yourself. 

HTTP Header Authentication
--------------------------


JWT Authentication
------------------


Man-In-The-Middle
-----------------


What is an API?
===============
An API is a contract:

Request > Defines how it expects to be used

Response > Defines what you can expect to receive when making a request

Typically an API request gives access to data whilst abstracting away the implementation. A rest API request may be supported in many languages. Often an HTTP request using curl, is supported, with data being received in JSON format. An API may be limited to CRUD; create, read, update, and delete. This means that if you want to create a new record, you use “POST.” If you are trying to read a record, you use “GET.” Updating may apply “PUT” or “PATCH.” While, to delete a record, use “DELETE.”

.. topic:: Put v Patch

	PUT overrides the record i.e. resets (any fields are not included in the PUT push are removed) whereas PATCH updates a record based on the information provided, leaving any fields not updated, intact.

.. warning:: Look for future-proofed APIs

	Favour APIs that utilise the Content-Type header. By reading the what format of data users are sending, the response may be provided in that same format.  For example, if you send a request using text/xml, you would hope to receive an XML response; whereas another user making a json request could be provided with with JSON.

	APIs that pay attention to this from the start are able to add as new formats become available.

.. topic:: Helper Libraries

	Many providers of APIs supply a software development kit (SDK), or helper libraries. This is what GCP has done to allow you to interact with their APIs. The library allows the server of the API to abstract away much of the code required to make requests to the API.

	When you make requests using cloud shell, you are interacting with the SDK that Google has set up to make interacting with their APIs simpler.


##############
Enabling APIs
##############

GCP's services are available through their API services. From creating a VM, to a Cloud Storage bucket — you are using API functions in the background.

.. topic:: Enable APIs in the GCP
	
	GCP > APIs & Services

	most APIs are not enabled by default.

	If you attempt an operation that requires a disabled API, then you may be prompted to enable it.

	From this console you may click on an API name to view details about its usage.

	You search for APIs that you want to administer through an interface that resembles a Google market place.

