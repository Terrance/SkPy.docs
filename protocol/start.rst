Getting started
===============

Terminology
-----------

- **connected account** -- user currently authenticated to the API
- **user identifier** -- user's Skype username, e.g. ``fred.2``
- **prefix** -- conversation type: ``1`` = Messenger(?), ``8`` = user, ``19`` = group, ``28`` = internal (e.g. ``28:concierge``)
- **chat identifier** -- unique chat name, e.g. ``a1b2c3d4...@thread.skype``
- **thread identifier** -- combination of prefix and user/chat identifier, e.g. ``8:fred.2``, ``19:a1b2c3d4...@thread.skype``
- **blob** -- old-style group chat identifier (and only identifier for P2P chats)

Endpoints
~~~~~~~~~

An endpoint represents a single connection between Skype and a client.  A user may have multiple endpoints connected, for example if running Skype on their desktop and their phone.  Similarly, connecting through SkPy creates a new endpoint.

There is a noticeable difference between being connected (at least one open endpoint) and being available (at least one client set status to *Online*).  Skype for Web makes the distinction by showing an empty white circle when all clients claim to be offline, but no circle at all for no endpoints connected.

For desktop clients, the *Invisible* status sets its endpoint to be offline, whereas *Offline* actually disconnects the endpoint from the network.

API tokens
----------

See :ref:`logging-in` for how to obtain tokens.  Unless otherwise noted, authentication is handled as follows:

- APIs on ``client-s.gateway.messenger.live.com`` (or an alternative subdomain, see :ref:`registration-token`) require registration token authentication using the ``RegistrationToken`` header.
- APIs on ``api.asm.skype.com`` take an ``Authorization`` header of the form ``skype_token <token>``.
- All other APIs take an ``X-SkypeToken`` header set to the Skype token.

.. _pagination:

Pagination
----------

Some APIs, such as fetching recent conversations or messages, include a ``syncState`` URL in the response.  This allows you to fetch subsequent pages of data by calling this URL rather than the documented one.

Typically (for recents), the first call will give you the 10 most recent objects of that type.  The response from the provided ``syncState`` URL would be objects 11 to 20, and a new ``syncState`` URL for objects 21 to 30, and so on.

If a new object becomes available in the meantime, it is provided in the next synced call.  For example, assume we start on item 100.  The first call will provide items 100 to 91 (the 10 most recent).  The next call gives 90 to 81.  Two more items show up, 101 and 102.  The next call will now give 102, 101, and 80 to 73.

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
