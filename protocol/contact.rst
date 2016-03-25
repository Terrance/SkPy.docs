Contact list
============

.. http:get:: https://contacts.skype.com/contacts/v1/users/(string:id)/contacts

    This provides a collection of users, both contacts (where the current user has accepted the contact's auth request, or sent one to them), and suggestions (users suggested by Skype but are not currently contacts) -- the latter have their ``suggested`` property set to ``true``.

    :param id: user identifier of connected account
    :resjson authorized:
    :resjson blocked:
    :resjson display_name:
    :resjson id:
    :resjson locations:
    :resjson name:
    :resjson type:

.. http:get:: https://api.skype.com/users/(string:id)/profile

    Returns the full profile for the given user, with both public and contact-private fields.  The current user is only authorised to request information for users in their contact list.

    :param id: user identifier of contact
    :resjson about:
    :resjson avatarUrl:
    :resjson birthday:
    :resjson city:
    :resjson country:
    :resjson emails:
    :resjson firstname:
    :resjson gender:
    :resjson homepage:
    :resjson jobtitle:
    :resjson language:
    :resjson lastname:
    :resjson mood:
    :resjson phoneHome:
    :resjson phoneMobile:
    :resjson phoneOffice:
    :resjson province:
    :resjson richMood:
    :resjson username:

.. http:post:: https://api.skype.com/users/self/contacts/profiles

    Retrieves public information for any user, regardless of contact status.

    :form contacts[]: user identifiers
    :resjsonarr avatarUrl:
    :resjsonarr city:
    :resjsonarr country:
    :resjsonarr displayname:
    :resjsonarr firstname:
    :resjsonarr lastname:
    :resjsonarr mood:
    :resjsonarr richMood:
    :resjsonarr username:

Skype directory
---------------

.. http:get:: https://api.skype.com/search/users/any

    Search the Skype directory for users.

    :query keyWord: string to search for
    :query contactTypes[]: ``skype``
    :resjsonarr ContactCards: objects containing public profile information

Auth requests
-------------

.. http:get:: https://contacts.skype.com/contacts/v1/users/self/contacts/auth-request

    Any pending auth requests sent from other users to the current user will be returned here.

    :resjsonarr sender:
    :resjsonarr greeting:

.. http:put:: https://contacts.skype.com/contacts/v1/users/self/contacts/auth-request/(string:id)/(string:action)

    Respond to an auth request.  Note that accepting a request does not add the user to the current user's contacts, this must be done in a separate request.  This also means that auth status is separate from appearing in the other user's contact list.

    :param id: user identifier of requesting user
    :param action: either ``accept`` or ``decline``

.. http:put:: https://client-s.gateway.messenger.live.com/v1/users/ME/contacts/(string:id)

    Add a user to the current user's contact list.  As above, this has no effect on auth status.

    :param id: user thread identifier of not-yet-contact

.. http:delete:: https://client-s.gateway.messenger.live.com/v1/users/ME/contacts/(string:id)

    Remove a user from the current user's contact list.  As above, this has no effect on auth status.

    :param id: user thread identifier of contact
