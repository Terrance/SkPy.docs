Home
====

Here you will find the documentation for `SkPy <https://pypi.org/project/SkPy/>`_, an unofficial Python library for interacting with the Skype HTTP API.

Features
--------

The aim of this library is to provide feature-complete support for Skype for Web.  So far, it supports:

- contacts: retrieving the contact list and groups, sending and responding to invites, searching the directory
- conversations: sending text messages, marking read, rich text formatting, file/image transfers, shared contacts
- group chats: creating new conversations, adding/removing members, delegating admins, setting topic/history, join URLs
- events: receiving conversation messages, status and endpoint changes
- translation API, user settings, credit/subscription info and more

.. warning::
    The upstream APIs used here are undocumented and are liable to change, which may cause parts of this library to fall apart in obvious or non-obvious ways.

    If you're looking to create a bot for other Skype users to interact with, consider using the official `Microsoft Bot Framework <https://dev.botframework.com>`_ instead.

Note that the protocol and APIs are not feature-complete with other Skype clients -- the :ref:`Skype protocol` pages have various notes on what is and isn't available over the HTTP APIs.

Contributing
------------

Take a look at the `GitHub repository <https://github.com/Terrance/SkPy>`_ for how to get involved.

Before raising an issue or pull request:

- Checkout the latest version of the code to ensure the problem hasn't already been solved.
- Make sure you're not encountering a server-side issue, particularly with authentication.  Try logging into `Skype for Web <https://web.skype.com>`_ to see where the problem lies.
- Set ``SKPY_DEBUG_HTTP=1`` in your environment to output all HTTP requests between the library and Skype's APIs.
- Include some sample code to reproduce a problem, along with a full traceback and HTTP output if relevant.

The documentation (both for SkPy, and the Skype for Web protocol) is a work in progress, but the content is `also hosted on GitHub <https://github.com/Terrance/SkPy.docs>`_ -- submissions welcome.

Contents
--------

.. toctree::

    guides/index
    reference/index
    background/index
