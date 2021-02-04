Getting started
===============

Endpoints
---------

An endpoint represents a single connection between Skype and a client.  A user may have multiple endpoints connected, for example if running Skype on their desktop and their phone.  Similarly, connecting through SkPy creates a new endpoint.

There is a noticeable difference between being connected (at least one open endpoint) and being available (at least one client set status to *Online*).  Skype for Web makes the distinction by showing an empty white circle when all clients claim to be offline, but no circle at all for no endpoints connected.

For desktop clients, the *Invisible* status sets its endpoint to be offline, whereas *Offline* actually disconnects the endpoint from the network.

API tokens
----------

See :ref:`logging-in` for how to obtain tokens.  Unless otherwise noted, authentication is handled as follows:

- APIs on ``client-s.gateway.messenger.live.com`` (or an alternative subdomain, see :ref:`registration-token`) require registration token authentication using the ``RegistrationToken`` header.
- APIs on ``api.asm.skype.com`` take an ``Authorization`` header of the form ``skype_token <token>``.
- All other APIs take an ``X-SkypeToken`` header set to the Skype token.

Unsupported features
--------------------

P2P group chats
~~~~~~~~~~~~~~~

These are the older variants of group conversations, referenced by blob rather than thread ID, and not stored centrally on Skype's servers.  As such, these are not available via Skype for Web.

.. note:: You can "convert" a P2P chat to a threaded conversation from within a supported client, by using the ``/fork`` command.  This creates a new cloud group chat with the same participants.

Multiple file transfers
~~~~~~~~~~~~~~~~~~~~~~~

It appears that file transfers involving more than one file are handled differently within downloadable clients, and are not yet available over Skype for Web (the message is replaced with "Receiving files over P2P network is not supported on Skype for Web").

`This forum post <https://community.skype.com/t5/Skype-for-Web-Beta/Skype-for-web-not-recieving-files-in-cloud-based-converstation/td-p/4307232>`_ notes that there are two file transfer modes, one of which is "cloud transfer" and works with Skype for Web.  Other clients will likely be updated to support this at some point.

.. _pagination:

Pagination
----------

Some APIs, such as fetching recent conversations or messages, include ``syncState`` URLs in the response.  This allows you to fetch subsequent pages of data by calling this URL rather than the documented one.

.. code-block:: javascript

    {...
     "_metadata": {"backwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/...?syncState=...&view=msnp24Equivalent",
                   "forwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/...?syncState=...&view=msnp24Equivalent",
                   "syncState": "https://db3-client-s.gateway.messenger.live.com/v1/...?syncState=...&view=msnp24Equivalent",
                   "totalCount": 10},
     ...}

The ``backwardLink`` and ``forwardLink`` attributes link to the previous and next result set, whilst ``syncState`` is the current page URL, and the one needed to retrieve all results.

Typically (for recents), the first call will give you the 10 most recent objects of that type.  The response from the provided ``syncState`` URL would be objects 11 to 20, and a new ``syncState`` URL for objects 21 to 30, and so on.  If a new object becomes available in the meantime, it is provided in the next synced call.  For example, assume we start on item 100.  The first call will provide items 100 to 91 (the 10 most recent).  The next call gives 90 to 81.  Two more items show up, 101 and 102.  The next call will now give 102, 101, and 80 to 73.

Message formatting
------------------

Different clients seem to support varying amounts of HTML formatting tags, as shown in this compatibility table:

=====================================================  =============  =======  ===  =======
Code                                                   Skype for Web  Windows  Mac  Android
=====================================================  =============  =======  ===  =======
``<b>Bold</b>``                                        Yes            Yes      Yes  Yes
``<i>Italic</i>``                                      Yes [1]_       Yes      Yes  Yes
``<u>Underline</u>``                                   No             Yes      No   No
``<s>Strikethrough</s>``                               Yes [1]_       Yes      Yes  Yes
``<font color="#ff0000">Colour</font>``                Yes            Yes      No   Yes
``<font size="24">Size</font>``                        No             Yes      No   No
``<blink>Blink</blink>``                               No             Yes      No   No
``<center>Centre</center>``                            No             Yes      Yes  No
``<a href="http://google.com">http://google.com</a>``  Yes            Yes      Yes  Yes
``<a href="http://google.com">Custom link</a>`` [2]_   Yes            Yes      Yes  Yes
``<pre>Preformatted</pre>``                            Yes [1]_       Yes      Yes  Yes
=====================================================  =============  =======  ===  =======

.. [1] Only works if the correct ``raw_pre`` and ``raw_post`` attributes are specified.

.. [2] Skype may block sending the message at server level (error message "Failure due to: BlockedContent") if it impersonates another link, e.g. ``<a href="http://youtube.com">http://google.com</a>``.

Response codes
--------------

- HTTP 429, error code 803: auth rate limit exceeded (-5 minute cooldown)
- HTTP 404, error code 729: no endpoint created (need to refresh registration token)
