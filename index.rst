Home
====

Here you will find the documentation for `SkPy <https://pypi.python.org/pypi/SkPy>`_, an unofficial Python library for interacting with the Skype HTTP API.

Here be dragons
---------------

The upstream APIs used here are undocumented and are liable to change, which may cause parts of this library to fall apart in obvious or non-obvious ways.  You have been warned.

Features
--------

The aim of this library is to provide feature-complete support for Skype for Web.  So far, it supports:

- contacts: retrieving the contact list and groups, sending and responding to invites, searching the directory
- conversations: sending text messages, marking read, rich text formatting, file/image transfers, shared contacts
- group chats: creating new conversations, adding/removing members, delegating admins, setting topic/history, join URLs
- events: receiving conversation messages, status and endpoint changes
- and more: translation API, user settings, credit/subscription info

Note that the protocol and APIs are not feature-complete with other Skype clients -- the :ref:`protocol <skype-protocol>` pages have various notes on what is and isn't available over the HTTP APIs.

Requirements
------------

- Python 2.6+ (includes 3.x)
- `BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_
- `Requests <http://www.python-requests.org/en/latest/>`_ [1]_
- `Responses <https://github.com/getsentry/responses>`_ (for tests)

.. [1] Note that Requests no longer supports Python 3.2 -- the last working version is 2.10.0.

Contributing
------------

Take a look at the `GitHub repository <https://github.com/OllieTerrance/SkPy>`_ for how to get involved.

Before raising an issue or pull request:

- Checkout the latest version of the code to ensure the problem hasn't already been solved.
- Make sure you're not encountering a server-side issue, particularly with authentication.  Try logging into `Skype for Web <https://web.skype.com>`_ to see where the problem lies.
- Set ``SKPY_DEBUG_HTTP=1`` in your environment to output all HTTP requests between the library and Skype's APIs.
- Include some sample code to reproduce a problem, along with a full traceback and HTTP output if relevant.

The documentation (both for SkPy, and the Skype for Web protocol) is a work in progress, but the content is `also hosted on GitHub <https://github.com/OllieTerrance/SkPy.docs>`_ -- submissions welcome.

Contents
--------

.. toctree::

    self
    usage
    api
    internal
    protocol/index
