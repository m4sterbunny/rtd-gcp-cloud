===============
What is an API?
===============

An API is a contract:

Request > Defines how it expects to be used

Response > Defines what you can expect to receive when making a request

Typically an API request gives access to data whilst abstracting away the implementation. A rest API request may be supported in many languages. Often an HTTP request using curl, is supported, with data being received in JSON format. An API should be limited to CRUD; create, read, update, and delete. This means that if you want to create a new record, you use “POST.” If you are trying to read a record, you use “GET.” Updating may apply “PUT” or “PATCH.” While, to delete a record, use “DELETE.”

.. note:: Put v Patch

	PUT overrides the record (any fields are not included in the PUT push are removed) whereas PATCH updates a record based on the information provided, leaving any fields not updated intact.

.. warning:: Look for future-proofed APIs

	Favour APIs that utilise the Content-Type header. By reading the what format of data users are sending, the response may be provided in that same format.  For example, if you send a request using text/xml, you would hope to receive an XML response; whereas another user making a json request could be provided with with JSON.

	APIs that pay attention to this from the start are able to add as new formats become available.


##############
Enabling APIs
##############

Programatic access of GCP's services is available through their API services. From creating a VM, to a Cloud Storage bucket- you are using API functions in the background.

.. topic:: Enable APIs
	
	GCP > APIs & Services

	most APIs are not enabled by default.

	If you attempt an operation that requires a disabled API, then you may be prompted to enable it.

	From this console you may click on an API name to view details about its usage.

You search for APIs that you want to administer through an interface that resembles a google market place.

