Other APIs
==========

Web UI options
--------------

Skype for Web allows configuring various server-side settings for the connected account.  This table shows how UI options map to flags and options:

======================================  =============  =======================
Setting                                 Value          Flags/options
======================================  =============  =======================
*Notifications tab*
------------------------------------------------------------------------------
Notifications                           ON             21=OFF
Sounds                                  ON             22=OFF
*Messaging tab*
------------------------------------------------------------------------------
Web link previews                       ON             11=OFF
YouTube player                          ON             12=ON
@mention notifications                  ON             13=OFF
Enable image paste                      ON             14=ON
Typing indicator                        ON             20=OFF
Emoticon suggestions                    ON             23=ON
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

Profile and services
--------------------

The people settings endpoint provides another interface for setting account-wide options.  Currently the only known one here is ``Skype.AutoBuddy``, for automatically adding address book contacts to the Skype contact list.

.. http:get:: https://people.directory.live.com/people/account/settings

    Retrieve all set options.

    .. note:: ``Skype.AutoBuddy`` is only returned if true.  The value is also returned as a string.

    :reqheader X-AppId: ``5c7a1e34-3a23-4a36-b2e6-7aa15be85f07``
    :reqheader X-SerializeAs: ``purejson``

    .. code-block:: javascript

        {"Settings": [{"Name": "Skype.AutoBuddy", "Value": "true"}]}

.. http:post:: https://people.directory.live.com/people/account/settings

    Update one or more options.

    .. note:: Boolean values are passed as booleans here, despite being retrieved as a string.

    :reqheader X-AppId: ``5c7a1e34-3a23-4a36-b2e6-7aa15be85f07``
    :reqheader X-SerializeAs: ``purejson``
    :reqjsonarr Settings: subset of data to add or edit

The profile provides access to contact email addresses and phone numbers on the account.

.. http:get:: https://pf.directory.live.com/profile/mine/System.ShortCircuitProfile.json

    Retrieve all profile information for the connected account.

    :reqheader PS-ApplicationId: ``5c7a1e34-3a23-4a36-b2e6-7aa15be85f07``

    .. code-block:: javascript

        {"TraceGraph": null,
         "Views": [{"Attributes": [{"Acl": null,
                                    "Name": "PersonalContactProfile.Emails",
                                    "Value": [{"Acl": null,
                                               "AddSearchableApplications": null,
                                               "DeleteSearchableApplications": null,
                                               "HasSearchableApplications": true,
                                               "Label": "Email_Other",
                                               "Name": "fred.adams@live.co.uk",
                                               "Searchable": true,
                                               "SearchableApplications": [{"Name": "Skype"}],
                                               "Source": "Msa",
                                               "State": "Verified"}]},
                                   {"Acl": null,
                                    "Name": "PersonalContactProfile.Emails.LastModified",
                                    "Value": "/Date(1451606400000)/"},
                                   {"Acl": null,
                                    "Name": "PhoneVerificationQosAlert",
                                    "Value": 0},
                                   {"Acl": null,
                                    "Name": "PersonalContactProfile.Phones",
                                    "Value": [{"Acl": "",
                                               "AddSearchableApplications": null,
                                               "Country": "UK-44",
                                               "CountryName": "UK",
                                               "DeleteSearchableApplications": null,
                                               "HasSearchableApplications": false,
                                               "Label": "Phone_Other",
                                               "Name": "07012345678",
                                               "Searchable": false,
                                               "SearchableApplications": [],
                                               "Source": "Msa",
                                               "State": "Verified",
                                               "SuggestedVerifyMethod": "Sms"}]}],
                    "Id": {"Cid": "-9000000000000000000", "Puid": null}}]}

.. http:post:: https://pf.directory.live.com/profile/mine/System.ShortCircuitProfile.json

    Make changes to a part of the profile information.

    :reqheader PS-ApplicationId: ``5c7a1e34-3a23-4a36-b2e6-7aa15be85f07``
    :reqjsonarr Attributes: subset of data to add or edit

Services are additional or paid featured applied to an account, such as voicemail, local numbers, and Skype minutes.

.. http:get:: https://consumer.entitlement.skype.com/users/(string:id)/services

    Retrieve a list of all active services.

    :query id: user identifier of connected account
    :reqheader Accept: ``application/json; ver=3.0``

    .. code-block:: javascript

        [{"active": false,
          "attributes": {"currency": "GBP"},
          "balance": 0,
          "balanceFormatted": "£0.00",
          "end": null,
          "href": "/users/fred.2/services/pstn",
          "id": "pstn",
          "reset": null,
          "service": "pstn",
          "start": null},
         {"active": true,
          "end": "2036-01-01T00:00:00+00:00",
          "href": "/users/fred.2/services/voicemail",
          "id": "voicemail",
          "reset": null,
          "service": "voicemail",
          "start": "2016-01-01T00:00:00+00:00"},
         {"active": true,
          "end": "2016-01-01T00:00:00+00:00",
          "href": "/users/fred.2/services/pstn_transfer",
          "id": "pstn_transfer",
          "reset": null,
          "service": "pstn_transfer",
          "start": "2016-01-01T00:00:00+00:00"},
         {"active": true,
          "attributes": {"monthly_minutes": 60,
                         "package": "api_300-region-landline-world-60"},
          "balance": 60,
          "data": {"calling_plan": "api_300-region-landline-world",
                   "href": "/offers/calling-legacy/skus/package-api_300-region-landline-world-60/subscriptions/package-api_300-region-landline-world-60-1m",
                   "nameFormatted": "World minutes for Office 365 60 mins 1 month"},
          "end": "2017-01-01T00:00:00+00:00",
          "href": "/users/fred.2/services/package.api_300-region-landline-world-60",
          "id": "package.api_300-region-landline-world-60",
          "quota": 60,
          "reset": "2016-02-01T00:00:00+00:00",
          "service": "package",
          "start": "2016-01-01T00:00:00+00:00"}]

.. http:get:: https://consumer.entitlement.skype.com/users/(string:id)/services/(string:service)

    Fetch details for a single service.

    :query id: user identifier of connected account
    :query service: active service identifier
    :reqheader Accept: ``application/json; ver=3.0``

    .. code-block:: javascript

        {"active": false,
         "attributes": {"currency": "GBP"},
         "balance": 0,
         "balanceFormatted": "£0.00",
         "end": null,
         "href": "/users/fred.2/services/pstn",
         "id": "pstn",
         "reset": null,
         "service": "pstn",
         "start": null}

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
