Conversations
=============

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations

    This returns an array of conversations that the current user has most recently interacted with.

    .. note:: This endpoint is :ref:`paginated <pagination>`.

    :query startTime: ``0``
    :query view: ``msnp24Equivalent``
    :query targetType: ``Passport|Skype|Lync|Thread``

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)

    Retrieve details about a conversation.

    :param id: chat thread identifier

Group conversations
-------------------

.. http:get:: https://client-s.gateway.messenger.live.com/v1/threads/(string:id)

    Fetch additional group-specific information, including the members and admins of the chat, topic, and join permissions.

    :param id: chat thread identifier

.. http:post:: https://client-s.gateway.messenger.live.com/v1/threads

    Create a new group conversation.

    Each member object consists of an ``id`` (user thread identifier), and role (either ``Admin`` or ``User``).

    :reqjson members: array of member objects
    :resheader Location: URL for the new conversation

.. http:put:: https://client-s.gateway.messenger.live.com/v1/threads/(string:id)/properties

    Update properties of a group conversation.

    :param id: chat thread identifier
    :reqjson name: name of parameter to be updated (from the rest of this list)
    :reqjson topic: new conversation topic
    :reqjson joiningenabled: whether users can join by URL
    :reqjson historydisclosed: whether newly-joining users can see past message history

Join URLs
---------

.. http:post:: https://api.scheduler.skype.com/threads

    Retrieve the join URL for a group conversation, if it is currently public.

    :reqjson baseDomain: ``https://join.skype.com/launch/``
    :reqjson threadId: chat thread identifier
    :resjson JoinUrl: public join URL

.. http:post:: https://join.skype.com/api/v2/conversation/

    Convert a join URL into standard identifiers.

    .. note:: No authentication is required for this endpoint.

    :reqjson shortId: join identifier from the URL
    :reqjson type: ``wl``
    :resjson Resource: chat thread identifier
    :resjson Id: long form identifier
    :resjson ChatBlob: thread blob (old-style identifier)

Messages
--------

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Retrieve the most recent messages from the conversation.

    .. note:: This endpoint is :ref:`paginated <pagination>`.

    :param id: chat thread identifier

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Send a message to the conversation.  There are several additional parameters that can be passed in for different message types.

    :param id: chat thread identifier
    :reqjson contenttype: ``text``
    :reqjson messagetype: base message type
    :reqjson content: raw content for the message

.. http:delete:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Delete all message history for this client.

    :param id: chat thread identifier
