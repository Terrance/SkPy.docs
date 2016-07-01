Other APIs
==========

Web UI options
--------------

Skype for Web allows configuring various server-side settings for the connected account.  This table shows how UI options map to flags and options:

======================================  =============  =======================
Setting                                 Value          Flags/options
======================================  =============  =======================
*Messaging tab*
------------------------------------------------------------------------------
Web link previews                       ON             11=OFF
YouTube player                          ON             12=ON
@mention notifications                  ON             13=OFF
Enable image paste                      ON             14=ON
Typing indicator                        ON             20=OFF
*Privacy tab*
------------------------------------------------------------------------------
Allow calls from... [1]_                Anyone         OPT_SKYPE_CALL_POLICY=0
\                                       Contacts only  OPT_SKYPE_CALL_POLICY=2
Allow video and screen sharing from...  Anyone         15=OFF, 16=OFF
\                                       Contacts only  15=ON,  16=OFF
\                                       Nobody         15=OFF, 16=ON
======================================  =============  =======================

.. [1] This setting uses the named options APIs, all others use flags.

.. http:get:: https://flagsapi.skype.com/api/v1

    Retrieve all flags set for the current user.

    .. code-block:: javascript

        [1, 12, 14, 15]

.. http:put:: https://flagsapi.skype.com/api/v1/(int:id)

    Set (enable) a flag.

    :param id: flag identifier

.. http:delete:: https://flagsapi.skype.com/api/v1/(int:id)

    Clear (disable) a flag.

    :param id: flag identifier

.. http:get:: https://api.skype.com/users/(str:id)/options/(str:option)

    Get an option's value.  One of ``optionInt``, ``optionBin`` or ``optionStr`` will be set in the response.

    :param id: user identifier of connected account
    :param option: option name

    .. code-block:: javascript

        {"optionInt": 0, "optionName": "OPT_SKYPE_NAME", "optionBin": null, "optionStr": null}

.. http:post:: https://api.skype.com/users/(str:id)/options/(str:option)

    Set an option's value.

    .. note:: There are probably ``stringValue`` and ``binaryValue`` form fields too, but no options of those types currently exist.

    :param id: user identifier of connected account
    :param option: option name
    :form integerValue: new value to set

URL scanning
------------

On Skype for Web, URLs in messages are displayed as rich block links containing a thumbnail and blurb.

.. http:get:: https://urlp.asm.skype.com/v1/url/info

    :query url: address to ping for info

    .. code-block:: javascript

        {"category": "generic",
         "content_type": "text/html",
         "description": "Search the world's information, including webpages, images, videos and more.",
         "favicon": "https://eus1-urlp.secure.skypeassets.com/static/google-32x32.ico",
         "favicon_meta": {"height": 32, "width": 32},
         "site": "www.google.com",
         "size": "-1",
         "status_code": "200",
         "thumbnail": "https://eus1-urlp.secure.skypeassets.com/static/google-90x90.png",
         "thumbnail_meta": {"height": 90, "width": 90},
         "title": "Google",
         "url": "http://google.com/",
         "user_pic": ""}

Static resources
----------------

Skype provides a single JSON file containing all emoticons, animations and videos.  Each release (i.e. when any resources are added or removed) has a different hash.

.. note:: SkPy currently comes bundled with emoticons from hash ``21280e53cdb24cde94cf4d4d0f5cb7c7`` (shortly after Christmas emoticons were added).

.. http:get:: https://static-asm.secure.skypeassets.com/pes/v1/configs/(string:hash)/views/en

    Retrieve a list of all resources, along with their shortcuts and visibility.

    :param hash: version identifier for the static set

Tracking
--------

There appears to be several analytics/tracking tools in place on Skype for Web, from the following domains:

- ``browser.pipe.aria.microsoft.com``
- ``c1.microsoft.com``
- ``go.trouter.io`` and ``*.drip.trouter.io``
- ``pipe.skype.com``
