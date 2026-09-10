Usage Guide
===========

This document covers command-line execution, environment variable configuration, and Python SDK usage for ``bib-ami``.

Command-Line Interface
----------------------

.. code-block:: bash

   bib-ami launch [--url <custom_url>]
   bib-ami lookup <identifier>

Environment Variables
---------------------

- ``BIB_AMI_API_URL``: Custom API endpoint (default: ``http://127.0.0.1:8000/v1``).
- ``BIB_AMI_WEB_URL``: Custom web application target (default: ``https://bib-ami.com``).
- ``BIB_AMI_API_KEY``: Bearer token for authenticated API requests.

Python SDK Usage
----------------

.. code-block:: python

   from bib_ami.client import BibAmiClient

   client = BibAmiClient()
   result = client.lookup_citation("10.1038/nature12345")
