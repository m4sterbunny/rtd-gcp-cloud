.. _HTTPheader: /cloud.google.com/sdk/
.. _here: https://martinfowler.com/articles/richardsonMaturityModel.html

.. toctree::
   :maxdepth: 2
   :caption: Contents:

===============
A World of API
===============

Application Programming Interfaces (APIs) busy fufilling promises everywhere!

APIs took databases and services into the cloud. Hitting an endpoint can bring you a hit:full-on-service ratio that you are going to love, perhaps some business administration. Or, you can simply store a photo.

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


What is an REST API?
====================

Representational state transfer -- the majority of RESTful APIs, if not all, use HTTP as a transport layer. Understandable choice, being that the infrastructure, servers, and client libraries for HTTP are widely available already. SMTP serves just as well, among others.

Many not RESTFull APIs also use HTTP for the same reason.

.. topic:: hint
	
	It is worth a pause to wonder. HTTP transports data for remote interactions, think tunnel. 


Back to APIs
------------

An API is a contract. Consider making a booking request, if the salon saves each request, the first interaction would be to POST your "document" say, the required date, to their exposed service endpoint (at a unique URI).

This POST triggers a response. The megrest response would be an automated message, say an error status. OR it could carry a payload, a document of its own, back down your (probably-HTTP) tunnel.

You may be given all available appointments for you to book, for example, probably as a JSON object. 

You book by posting back the appointment id for the one you want and bingo a 201 response comes with some kinda "resource added" success message, or some error handling alt, and you are off for a cut. 


.. topic:: Keep it simple stupid

	A method
		is called on an object
			providing argument

Poor design to our API, though -- let's fix that up a bit:

HTTP Verbs
----------

Your first step works as a POST and you will find many not-so-shiney APIs wrangling happily away like the example.

I prefer to see the actions or verbs made more precisely:

.. topic:: Get

	Make the request for the avaiable saloon slots with a GET request.

	GET is read only.
	Allows for caching.


Payloads
--------

So payloads don't just have to be the data in the objects stored in the system such as the available appointments. Payloads may offer the URI or path to perform the next action at. 

In our example we > Get > 200 response + Payload of a message body + > link for each timeslot for a > PUT to be made at. 

Job done we have a cut booked, again!

What a RElief!
--------------

By breaking services down into these URI endpoints, we build a service that is relatively bottle-neck free. No single service endpoint doing many jobs.

If devs stay altert human-readable tags such as the URI /base-address/linkrels/appoiontment/cancel, then an ever-dynamic set of services can be offered by an endpoint. 

more here_.


Expect to Conform
-----------------

If you are attemting to POST malformed data, expect to fail. Read the docs. API docs will explain how to present data to be sent. And, fit in with the security we started with.

Typically an API request gives access to data or a service whilst abstracting away the implementation. A rest API request may be supported in many languages. Often an HTTP request using curl, is supported, with data being passed in JSON format. An API may be limited to CRUD; create, read, update, and delete. This means that if you want to create a new record, you use “POST.” If you are trying to read a record, you use “GET.” Updating may apply “PUT” or “PATCH.” While, to delete a record, use “DELETE.”

.. topic:: Put v Patch

	PUT overrides the record i.e. resets (any fields are not included in the PUT push are removed) whereas PATCH updates a record based on the information provided, leaving any fields not updated, intact.

.. warning:: Look for future-proofed APIs

	Favour APIs that utilise the Content-Type header. By reading the what format of data users are sending, the response may be provided in that same format.  For example, if you send a request using text/xml, you would hope to receive an XML response; whereas another user making a json request could be provided with with JSON.

	APIs that pay attention to this from the start are able to add as new formats become available.

.. topic:: Helper Libraries

	Many providers of APIs supply a software development kit (SDK), or helper libraries. This is what GCP has done to allow you to interact with their APIs. The library allows the server of the API to abstract away much of the code required to make requests to the API.

	

##############
Enabling APIs
##############

GCP's services are available through their API services. From creating a VM, to a Cloud Storage bucket — you are using API functions in the background.

.. sidebar:: Console

	GCP > APIs & Services

.. topic:: Enable APIs in the GCP
	

	most APIs are not enabled by default.

	If you attempt an operation that requires a disabled API, then you may be prompted to enable it.

	From this console you may click on an API name to view details about its usage.

	You search for APIs that you want to administer through an interface that resembles a Google market place.

