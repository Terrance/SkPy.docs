Contact endpoints
=================

Contact list
------------

.. http:get:: https://contacts.skype.com/contacts/v2/users/(string:id)/contacts

    Retrieve a list of all users in the contact list, as well as suggestions from Skype.

    .. note::
        - The ``suggested`` and ``favorite`` fields only appear if set to ``true`` (that is, missing implies ``false``).
        - Contact entries for Skype bots will include an ``agent`` field that includes their permissions.
        - Changed from v1: user identifiers have moved to the ``mri`` key, and are now thread-prefixed.
        - Changed from v1: user-provided details (besides name) have moved to a nested ``profile`` object.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"contacts": [{"auth-certificate": "...",
                       "authorized": true,
                       "blocked": false,
                       "display_name": "Joe Bloggs",
                       "display_name_source": "user_edits",
                       "favorite": true,
                       "mri": "8:joe.4",
                       "person_id": "aaaaaaaa-0000-0000-0000-000000000000",
                       "profile": {"avatar_url": "https://api.skype.com/users/joe.4/profile/avatar?auth_key=...",
                                   "locations": [{"city": "London", "state": null, "country": "GB"}],
                                   "mood": "Happy <ss type=\"laugh\">:D</ss>",
                                   "name": {"first": "Joe", "surname": "Bloggs", "nickname": "Joe Bloggs"},
                                   "phones": [{"number": "+442099887766", "type": 0},
                                              {"number": "+442020900900", "type": 1},
                                              {"number": "+447711223344", "type": 2},
                                              ...]}},
                      {"authorized": false,
                       "blocked": true,
                       "display_name": "Anna Cooper",
                       "email_hashes": ["..."],
                       "mri": "8:anna.7",
                       "profile": {"name": {"first": "Anna", "surname": "Cooper"}},
                       "suggested": true},
                      ...],
         "count": 4,
         "scope": "full"}

.. http:get:: https://contacts.skype.com/contacts/v2/users/(string:id)/blocklist

    Retrieve a list of blocked users.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"blocklist": [{"mri": "8:anna.7"},
                       ...],
         "scope": "full"}

.. http:get:: https://contacts.skype.com/contacts/v2/users/(string:id)/groups

    Retrieve a list of contact groups defined by the user.  Also includes the _Favourites_ group.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"count": 3,
         "groups": [{"contacts": ["28:concierge"],
                     "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
                     "name": "Bots"},
                    {"contacts": ["8:joe.4"],
                     "id": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
                     "is_favorite": true,
                     "name": "Favourites"},
                    ...],
         "scope": "full"}

.. http:get:: https://contacts.skype.com/contacts/v2/users/(string:id)

    Retrieves all information on contacts and groups.  Combines the three requests above into a single response.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"blocklist": [...],
         "contacts": [...],
         "groups": [...]}

Authorisation
-------------

.. http:get:: https://contacts.skype.com/contacts/v2/users/(string:id)/invites

    Retrieves any pending contact requests.  A user may have multiple requests open.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"invite_list": [{"invites": [{"message": "Hi Fred, I'd like to add you as a contact.",
                                       "time": "2016-01-01T00:00:00.000Z"},
                                      ...],
                          "mri": "8:anna.7"}
                         ...]}

.. http:post:: https://contacts.skype.com/contacts/v2/users/(string:id)/contacts

    Send a contact request to a user.

    :param id: user identifier of connected account
    :json mri: thread identifier of requesting user
    :json greeting: message to include with the request

.. http:put:: https://contacts.skype.com/contacts/v2/users/(string:id)/invites/(string:otherId)/(string:action)

    Accepts an open contact request, authorising this user to send and receive messages, or declines it.

    :param id: user identifier of connected account
    :param otherId: thread identifier of requesting user
    :param action: either ``accept`` or ``decline``

.. http:delete:: https://contacts.skype.com/contacts/v2/users/(string:id)/contacts/(string:otherId)

    Removes the authorisation for a user, revoking their ability to send and receive messages.

    :param id: user identifier of connected account
    :param otherId: thread identifier of requesting user

.. http:put:: https://contacts.skype.com/contacts/v2/users/(string:id)/blocklist/(string:otherId)

    Block a user from sending messages and seeing presence.  Note that this a second form of authorisation, and doesn't affect the main auth status.

    :param id: user identifier of connected account
    :param otherId: thread identifier of requesting user
    :json ui_version: ``skype.com``
    :json report_abuse: ``true`` if the user should be reported to Skype

.. http:delete:: https://contacts.skype.com/contacts/v2/users/(string:id)/blocklist/(string:otherId)

    Unblock a previously blocked user.

    :param id: user identifier of connected account
    :param otherId: thread identifier of requesting user

.. http:put:: https://client-s.gateway.messenger.live.com/v1/users/ME/contacts/(string:id)

    Add a user to the current user's contact list.  This has no effect on auth status, which must be approved by accepting an invite.

    :param id: user thread identifier of not-yet-contact

.. http:delete:: https://client-s.gateway.messenger.live.com/v1/users/ME/contacts/(string:id)

    Remove a user from the current user's contact list.  This has no effect on auth status, which must be revoked separately.

    :param id: user thread identifier of contact

Profile information
-------------------

.. http:get:: https://api.skype.com/users/(string:id)/profile

    Returns the full profile for the given user, with both public and contact-private fields.  The current user is only authorised to request information for users in their contact list.

    The value of ``id`` may be set to ``self``, to retrieve the same information as if using the current user's identifier.

    :param id: user identifier of contact

    .. code-block:: javascript

        {"about": "I am a Skype user.",
         "avatarUrl": "https://api.skype.com/users/joe.4/profile/avatar?auth_key=...",
         "birthday": "1987-01-02",
         "city": "London",
         "country": "GB",
         "emails": ["joe.bloggs@live.co.uk"],
         "firstname": "Joe",
         "gender": "1",
         "homepage": "http://www.joebloggs.com",
         "jobtitle": "Skype user",
         "language": "EN",
         "lastname": "Bloggs",
         "mood": "Happy :D",
         "phoneHome": "+442099887766",
         "phoneMobile": "+447711223344",
         "phoneOffice": "+442020900900",
         "province": "Greater London",
         "richMood": "Happy <ss type=\"laugh\">:D</ss>",
         "username": "joe.4"}

.. http:post:: https://api.skype.com/users/self/contacts/profiles

    Retrieves public information for any user, regardless of contact status.

    :form contacts[]: user identifiers

    .. code-block:: javascript

        [{"avatarUrl": "https://api.skype.com/users/anna.7/profile/avatar?cacheHeaders=1",
          "city": "Manchester",
          "country": "GB",
          "displayname": "Anna Cooper",
          "firstname": "Anna",
          "lastname": "Cooper",
          "mood": "Excited!",
          "richMood": "<i raw_pre=\"_\" raw_post=\"_\">Excited!</i>",
          "username": "anna.7"},
         ...]

Skype directory
---------------

.. http:get:: https://skypegraph.skype.com/search/v1.1/namesearch/swx/

    Search the Skype directory for users.

    :query searchstring: string to search for
    :query requestId: anything

    .. code-block:: javascript

        {"results": [{"nodeProfileData": {"about": "I am a Skype user.",
                                          "age": "29",
                                          "avatarUrl": "https://api.skype.com/users/joe.4/profile/avatar",
                                          "city": "London",
                                          "contactType": "Skype",
                                          "country": "United Kingdom",
                                          "countryCode": "gb",
                                          "gender": "1",
                                          "language": "en",
                                          "name": "Joe",
                                          "skypeId": "joe.4",
                                          "state": "Greater London"}},
                     ...]}

Bot users
---------

.. http:get:: https://api.aps.skype.com/v1/agents

    Retrieve information about a Skype bot user.  Without an identifier, retrieves all bots.

    :query agentId: UUID or username of the bot

    .. code-block:: javascript

        {"agentDescriptions": [{"agentId": "concierge",
                                "agentType": "Participant",
                                "capabilities": [],
                                "description": "This is a built-in certified Skype bot that will help you get the most from your Skype experience by providing tips and guidance.",
                                "developer": "Skype",
                                "displayName": "Skype",
                                "extra": "<a href=\"https://go.skype.com/tou\">Terms of Service</a><br/><a href=\"https://go.skype.com/privacy\">Privacy Statement</a>",
                                "isTrusted": true,
                                "privacyStatement": "https://go.skype.com/privacy",
                                "starRating": 5.0,
                                "supportedLocales": ["en-US", "en-GB"],
                                "tos": "https://go.skype.com/tou",
                                "userTileExtraLargeUrl": "https://az705183.vo.msecnd.net/dam/skype/media/concierge-assets/avatar/avatarcnsrg-800.png",
                                "userTileLargeUrl": "https://az705183.vo.msecnd.net/dam/skype/media/concierge-assets/avatar/avatarcnsrg-402.png",
                                "userTileMediumUrl": "https://az705183.vo.msecnd.net/dam/skype/media/concierge-assets/avatar/avatarcnsrg-144.png",
                                "userTileSmallUrl": "https://az705183.vo.msecnd.net/dam/skype/media/concierge-assets/avatar/avatarcnsrg-95.png",
                                "userTileStaticUrl": "https://az705183.vo.msecnd.net/dam/skype/media/concierge-assets/avatar/avatarcnsrg-144.png",
                                "webpage": "https://go.skype.com/faq.skype.bot"},
                               ...],
         "continuationToken": null}

Old endpoints
-------------

.. http:get:: https://contacts.skype.com/contacts/v1/users/(string:id)/contacts

    This provides a collection of users, both contacts (where the current user has accepted the contact's auth request, or sent one to them), and suggestions (users suggested by Skype but are not currently contacts) -- the latter have their ``suggested`` property set to ``true``.

    :param id: user identifier of connected account

    .. code-block:: javascript

        {"contacts": [{"authorized": true,
                       "avatar_url": "https://api.skype.com/users/joe.4/profile/avatar?auth_key=...",
                       "blocked": false,
                       "display_name": "Joe Bloggs",
                       "id": "joe.4",
                       "locations": [{"city": "London", "state": null, "country": "GB"}],
                       "mood": "Happy <ss type=\"laugh\">:D</ss>",
                       "name": {"first": "Joe", "surname": "Bloggs", "nickname": "Joe Bloggs"},
                       "phones": [{"number": "+442099887766", "type": 0},
                                  {"number": "+442020900900", "type": 1},
                                  {"number": "+447711223344", "type": 2},
                                  ...],
                       "type": "skype"},
                      {"authorized": false,
                       "blocked": false,
                       "display_name": "Anna Cooper",
                       "id": "anna.7",
                       "name": {"first": "Anna", "surname": "Cooper"},
                       "suggested": true,
                       "type": "skype"},
                      ...]}

.. http:get:: https://contacts.skype.com/contacts/v1/users/self/contacts/auth-request

    Any pending auth requests sent from other users to the current user will be returned here.

    .. code-block:: javascript

        [{"greeting": "Hi Fred Adams, I'd like to add you as a contact on Skype.",
          "sender": "anna.7"},
         ...]

.. http:put:: https://contacts.skype.com/contacts/v1/users/self/contacts/auth-request/(string:id)/(string:action)

    Respond to an auth request.  Note that accepting a request does not add the user to the current user's contacts, this must be done in a separate request.  This also means that auth status is separate from appearing in the other user's contact list.

    :param id: user identifier of requesting user
    :param action: either ``accept`` or ``decline``

.. http:get:: https://api.skype.com/search/users/any

    Search the Skype directory for users.

    :query keyWord: string to search for
    :query contactTypes[]: ``skype``

    .. code-block:: javascript

        [{"ContactCards": {"CurrentLocation": {"City": "London",
                                               "Country": "gb",
                                               "Province": "Greater London"},
                           "Skype": {"About": "I am a Skype user.",
                                     "Age": "29",
                                     "DisplayName": "Joe Bloggs",
                                     "Gender": "1",
                                     "Language": "en",
                                     "Rank": 0,
                                     "SkypeName": "joe.4"}}},
         ...]

Field notes
-----------

- Gender values are ``1`` for male, and ``2`` for female.
- Phone number types are ``0`` for home, ``1`` for work, and ``2`` for mobile.
- Location countries and languages are represented by two-letter country codes, though they are inconsistently cased -- some users have uppercase codes, others have lowercase.
- Whilst users have first and last name fields, the Skype desktop clients only provide a single name field.  As such, the names of many users are stored entirely in the first name field, with an empty last name.  In addition, some users' display names are blank regardless of the values of their name fields.

.. note::
    SkPy will automatically try to split names into the last name field if needed.
