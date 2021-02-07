Test classes
============

SkPy comes with some test cases that cover expectations from the server, and the correctness of the library's parsing.

Client testing
--------------

Here, responses from the server are mocked out and replaced with static strings, so the real Skype server is never contacted.  Test cases aim to ensure the library parses each response correctly.

.. automodule:: test.client

Server testing
--------------

These tests connect to the production Skype server to perform actions, checking that requests to and responses from the server are consistent with the library.

.. automodule:: test.server
