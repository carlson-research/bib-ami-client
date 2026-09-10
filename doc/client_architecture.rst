Client Architecture
===================

This document outlines the thin client architecture of ``bib-ami``.

Client-Server Model
-------------------

``bib-ami`` operates as a lightweight client that delegates heavy citation parsing, registry matching, and verification heuristics to the remote cloud engine.

SDK vs. Web Application
-----------------------

- **Web Application:** Interactive UI accessed via browser for visually managing and cleaning libraries.
- **CLI / SDK:** Lightweight client for terminal automation, script integration, and programmatic API access.
