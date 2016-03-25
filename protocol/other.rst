Other APIs
==========

Web UI options
--------------

Skype for Web allows setting some options, such as:

- ``11``: web link previews
- ``12``: YouTube players
- ``13``: @mention notifications
- ``14``: image paste

.. http:get:: https://flagsapi.skype.com/api/v1

    Retrieve all flags set for the current user.

.. http:put:: https://flagsapi.skype.com/api/v1/(int:id)

    Set (enable) a flag.

    :param id: flag identifier

.. http:delete:: https://flagsapi.skype.com/api/v1/(int:id)

    Clear (disable) a flag.

    :param id: flag identifier

URL scanning
------------

On Skype for Web, URLs in messages are displayed as rich block links containing a thumbnail and blurb.

.. http:get:: https://urlp.asm.skype.com/v1/url/info

    :query url: address to ping for info

Static resources
----------------

.. http:get:: https://static-asm.secure.skypeassets.com/pes/v1/configs/(string:hash)/views/en

    Retrieve a list of all emoticons and videos, along with shortcuts and visibility.  The hash changes each time the set of resources is changed.

    SkPy currently comes bundled with emoticons from hash ``21280e53cdb24cde94cf4d4d0f5cb7c7`` (shortly after Christmas emoticons were added).

    :param hash: version identifier for the static set

Tracking
--------

There appears to be several analytics/tracking tools in place on Skype for Web, from the following domains:

- ``browser.pipe.aria.microsoft.com``
- ``c1.microsoft.com``
- ``go.trouter.io`` and ``*.drip.trouter.io``
- ``pipe.skype.com``
