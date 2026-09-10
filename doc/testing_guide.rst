Testing Guide
=============

This document explains how to run tests for ``bib-ami`` using ``pytest`` and offline network mocking.

Running Tests
-------------

.. code-block:: bash

   pytest tests/

Offline Mocking
---------------

Use ``pytest-httpx`` to mock responses from the remote ``bib-ami`` API without making live HTTP requests during test runs.
