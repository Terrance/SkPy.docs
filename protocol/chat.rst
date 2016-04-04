Conversations
=============

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations

    This returns an array of conversations that the current user has most recently interacted with.  The ``lastMessage`` field holds a message object in the same format as retrieved from ``/v1/users/ME/conversations/(id)/messages``.

    .. note:: This endpoint is :ref:`paginated <pagination>`.

    :query startTime: ``0``
    :query view: ``msnp24Equivalent``
    :query targetType: ``Passport|Skype|Lync|Thread``

    .. code-block:: javascript

        {"_metadata": {"backwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations?syncState=...&view=msnp24Equivalent",
                       "forwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations?syncState=...&view=msnp24Equivalent",
                       "syncState": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations?syncState=...&view=msnp24Equivalent",
                       "totalCount": 10},
         "conversations": [{"id": "8:joe.4",
                            "lastMessage": {"clientmessageid": "1451606399999",
                                            "composetime": "2016-01-01T00:00:00.000Z",
                                            "content": "Hi!",
                                            "conversationLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/8:joe.4@thread.skype",
                                            "from": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:joe.4",
                                            "id": "1451606400000",
                                            "messagetype": "Text",
                                            "originalarrivaltime": "2016-01-01T00:00:00.000Z",
                                            "type": "Message",
                                            "version": "1451606400000"},
                            "messages": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/8:joe.4/messages",
                            "properties": {"clearedat": "1451606400000", "consumptionhorizon": "..."},
                            "targetLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:joe.4",
                            "type": "Conversation",
                            "version": 1451606400000},
                           {"id": "19:a0b1c2...d3e4f5@thread.skype",
                            "lastMessage": {"clientmessageid": "1451606399999",
                                            "composetime": "2016-01-01T00:00:00.000Z",
                                            "content": "A message for the team.",
                                            "conversationLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype",
                                            "from": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:anna.7",
                                            "id": "1451606400000",
                                            "messagetype": "Text",
                                            "originalarrivaltime": "2016-01-01T00:00:00.000Z",
                                            "type": "Message",
                                            "version": "1451606400000"},
                            "messages": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages",
                            "properties": {"consumptionhorizon": "..."},
                            "targetLink": "https://db3-client-s.gateway.messenger.live.com/v1/threads/19:a0b1c2...d3e4f5@thread.skype",
                            "threadProperties": {"lastjoinat": "1451606400000", "topic": "Team chat", "version": "1451606400000"},
                            "type": "Conversation",
                            "version": 1451606400000},
                           ...]}

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)

    Retrieve details about a conversation.

    :param id: chat thread identifier

    .. code-block:: javascript

        {"id": "19:a0b1c2...d3e4f5@thread.skype",
         "lastMessage": {"clientmessageid": "1451606399999",
                         "composetime": "2016-01-01T00:00:00.000Z",
                         "content": "A message for the team.",
                         "conversationLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype",
                         "from": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:anna.7",
                         "id": "1451606400000",
                         "messagetype": "Text",
                         "originalarrivaltime": "2016-01-01T00:00:00.000Z",
                         "type": "Message",
                         "version": "1451606400000"},
         "messages": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages",
         "properties": {"consumptionhorizon": "..."},
         "targetLink": "https://db3-client-s.gateway.messenger.live.com/v1/threads/19:a0b1c2...d3e4f5@thread.skype",
         "threadProperties": {"lastjoinat": "1451606400000", "topic": "Team chat", "version": "1451606400000"},
         "type": "Conversation",
         "version": 1451606400000}

Group conversations
-------------------

.. http:get:: https://client-s.gateway.messenger.live.com/v1/threads/(string:id)

    Fetch additional group-specific information, including the members and admins of the chat, topic, and join permissions.

    :param id: chat thread identifier

    .. code-block:: javascript

        {"id": "19:a0b1c2...d3e4f5@thread.skype",
         "members": [{"capabilities": [],
                      "cid": 0,
                      "friendlyName": "",
                      "id": "8:anna.7",
                      "linkedMri": "",
                      "role": "Admin",
                      "type": "ThreadMember",
                      "userLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/8:anna.7",
                      "userTile": ""},
                     {"capabilities": [],
                      "cid": 0,
                      "friendlyName": "",
                      "id": "8:joe.4",
                      "linkedMri": "",
                      "role": "User",
                      "type": "ThreadMember",
                      "userLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/8:joe.4",
                      "userTile": ""},
                     ...],
         "messages": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages",
         "properties": {"capabilities": ["AddMember",
                                         "ChangeTopic",
                                         "ChangePicture",
                                         "EditMsg",
                                         "CallP2P",
                                         "SendText",
                                         "SendSms",
                                         "SendFileP2P",
                                         "SendContacts",
                                         "SendVideoMsg",
                                         "SendMediaMsg",
                                         "ChangeModerated"],
                        "createdat": "1451606400000",
                        "creator": "8:anna.7",
                        "creatorcid": "0",
                        "historydisclosed": "true",
                        "joiningenabled": "true",
                        "picture": "URL@https://api.asm.skype.com/v1/objects/0-.../views/avatar_fullsize",
                        "topic": "Team chat"},
         "type": "Thread",
         "version": 1451606400000}

.. http:post:: https://client-s.gateway.messenger.live.com/v1/threads

    Create a new group conversation.

    Each member object consists of an ``id`` (user thread identifier), and role (either ``Admin`` or ``User``).

    :reqjson members: array of member objects
    :resheader Location: URL for the new conversation

.. http:put:: https://client-s.gateway.messenger.live.com/v1/threads/(string:id)/properties

    Update properties of a group conversation.  Only one property can be set at a time, which should be the value of the ``name`` field, and key for the field holding the new value.

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

    .. code-block:: javascript

        {"Blob": "AzByCx...XcYbZa",
         "Id": "Za0Yb1...By2Az3",
         "JoinUrl": "https://join.skype.com/<join-code>",
         "ThreadId": "19:a0b1c2...d3e4f5@thread.skype"}

.. http:post:: https://join.skype.com/api/v2/conversation/

    Convert a join URL into standard identifiers.

    .. note:: No authentication is required for this endpoint.

    :reqjson shortId: join identifier from the URL
    :reqjson type: ``wl``

    .. code-block:: javascript

        {"Action": "Chat",
         "ChatBlob": "AzByCx...XcYbZa",
         "FlowId": "1",
         "Id": "Za0Yb1...By2Az3",
         "Resource": "19:a0b1c2...d3e4f5@thread.skype"}

Messages
--------

.. http:get:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Retrieve the most recent messages from the conversation.

    .. note:: This endpoint is :ref:`paginated <pagination>`.

    :param id: chat thread identifier

    .. code-block:: javascript

        {"_metadata": {"backwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages?syncState=...&view=msnp24Equivalent",
                       "forwardLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages?syncState=...&view=msnp24Equivalent",
                       "syncState": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype/messages?syncState=...&view=msnp24Equivalent",
                       "totalCount": 10},
         "messages": [{"clientmessageid": "1451606399999",
                       "composetime": "2016-01-01T00:00:00.000Z",
                       "content": "A message for the team.",
                       "conversationLink": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/conversations/19:a0b1c2...d3e4f5@thread.skype",
                       "from": "https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:anna.7",
                       "id": "1451606400000",
                       "messagetype": "Text",
                       "originalarrivaltime": "2016-01-01T00:00:00.000Z",
                       "type": "Message",
                       "version": "1451606400000"},
                      ...]}

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Send a message to the conversation.  There are several additional parameters that can be passed in for different message types.

    :param id: chat thread identifier
    :reqjson contenttype: ``text``
    :reqjson messagetype: base message type
    :reqjson content: raw content for the message

    .. code-block:: javascript

        {"OriginalArrivalTime": 1451606400000}

.. http:delete:: https://client-s.gateway.messenger.live.com/v1/users/ME/conversations/(string:id)/messages

    Delete all message history for this client.

    :param id: chat thread identifier
