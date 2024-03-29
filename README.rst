
.. _HTTPheader: /cloud.google.com/sdk/
.. _GUIDE: http://udig.refractions.net/files/docs/latest/user/docguide/sphinxSyntax.html
.. _Sublime: https://sublime-and-sphinx-guide.readthedocs.io/en/latest/references.html
.. _ReadtheDocs: https://rtd-gcp-cloud.readthedocs.io/en/latest/

Hosted on ReadtheDocs_, this documentation was made to assist with the Google Cloud Engineer Course as per: https://help.pluralsight.com/help/andela-4-0-courses

currently using resources from https://app.pluralsight.com/paths/certificate/cloud-engineering-with-google-cloud

Like this simple site? Try this great rST guide:
------------------------------------------------

Guide_

or `Get Started <https://www.sphinx-doc.org/en/master/usage/quickstart.html>`_

What is rST?
------------

Restructured Text is a simple markup language to abstract away the look of the document from the content. 

rST is probably the slimmest markup language I have yet used (it pairs well with Sublime_). It is used to create a static page. It is an example of headless content management.

With a word processor, choices such as layout, landscape v portrait, for example come early. With rST the content and its look are separated. Look is defined by CSS templates.

The final look is controlled, to some extent, by simple tools from the rST CSS-control kit that work alongside the page content. Levels and styles such as headers are tagged using symbols, images may be referenced as with cousins HTML/XML. 

Even better, in terms of simplicity for reading, components may be labelled and called. All it requires is that you define the key pair at the start of the document, i.e. you declare the resource you will use. 

For example, this page linked to a guide. At the top of the source .rst page Guide is 'declared' and its location/reference, in this case a URL to a web page provided.


.. code-block::
	:linenos:

		.. Guide: <URL>


To trigger this key pair, Guide is called using:

.. code-block::
	:linenos:

		Guide_


This name==reference statment is not rendered, i.e., it is not visible in the final built page (HTML, pdf, etc.).


Where is the rST?
-----------------

Because rST files are going to generate your site's content, they are stored in the source folder.

The final documents sit in the build folder where the rST + CSS are rendered into an output such as HTML docs.


I like it show me more
----------------------

Nifty eh?

The key-pair, declare-it-at-the-top function allows you to grab data from files in all the ways you can think of. So, far folks use it to:

- single source content
- inject images
- inject data

and the like.

Images
------


Hosting rST on Read the Docs
============================

Welcome to a great free service offered by the Read the Docs team.