Events
======

One endpoint represents one client for the connecting user.  A user may have many endpoints connected at once, for example a laptop, a phone, a tablet and so on.  Each client and endpoint has its own availability status and event subscriptions.

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/endpoints/(string:endpoint)/active

    Endpoints must be kept alive by regularly pinging them.  Skype for Web does this roughly every 45 seconds, sending a timeout value of ``12``.

    :param endpoint: UUID of an active endpoint
    :json timeout: length of time to keep the endpoint alive

Event subscriptions
-------------------

Events provide real-time information for messages sent and received in conversations, as well as endpoint and presence changes.

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/endpoints/(string:endpoint)/subscriptions

    The list of resources, requested by Skype for Web when subscribing, are::

        /v1/threads/ALL
        /v1/users/ME/contacts/ALL
        /v1/users/ME/conversations/ALL/messages
        /v1/users/ME/conversations/ALL/properties

    Note that Skype for Web uses the special ``SELF`` endpoint, though this does not appear to be a requirement (the endpoint generated during registration token retrieval can also be used).

    :param endpoint: UUID of an active endpoint
    :reqjson interestedResources: list of endpoints to receive events for
    :reqjson template: ``raw``
    :reqjson channelType: ``httpLongPoll`` (``tcpSocket`` is also a valid option)

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/endpoints/(string:endpoint)/subscriptions/0/poll

    This returns an array of events since the last poll, under the ``eventMessages`` key.  If no events have occurred, the request will block (the connection will hang, waiting for the server) until an event occurs, at which point it is returned immediately.  After about 30 seconds with no events, an empty array is returned.

    :param endpoint: UUID of an active endpoint

    .. code-block:: javascript

        {'eventMessages': [{'id': 1000,
                            'resource': {'availability': 'Online',
                                         'capabilities': '',
                                         'endpointPresenceDocLinks': ['https://db3-client-s.gateway.messenger.live.com/v1/users/8:joe.4/endpoints/{...}/presenceDocs/messagingService',
                                                                      ...],
                                         'id': 'messagingService',
                                         'selfLink': 'https://db3-client-s.gateway.messenger.live.com/v1/users/8:joe.4/presenceDocs/messagingService',
                                         'status': 'Online',
                                         'type': 'UserPresenceDoc'},
                            'resourceLink': 'https://db3-client-s.gateway.messenger.live.com/v1/users/ME/contacts/8:joe.4/presenceDocs/messagingService',
                            'resourceType': 'UserPresence',
                            'time': '2016-01-01T00:00:00Z',
                            'type': 'EventMessage'},
                           {'id': 1001,
                            'resource': {'id': 'messagingService',
                                         'privateInfo': {'epname': 'Joe-Desktop'},
                                         'publicInfo': {'capabilities': '',
                                                        'nodeInfo': 'xx',
                                                        'skypeNameVersion': '908/1.22.117/swx-skype.com',
                                                        'typ': '1',
                                                        'version': '908/1.22.117'},
                                         'selfLink': 'https://db3-client-s.gateway.messenger.live.com/v1/users/8:joe.4/endpoints/{...}/presenceDocs/messagingService',
                                         'type': 'EndpointPresenceDoc'},
                            'resourceLink': 'https://db3-client-s.gateway.messenger.live.com/v1/users/8:joe.4/endpoints/{...}/presenceDocs/messagingService',
                            'resourceType': 'EndpointPresence',
                            'time': '2016-01-01T00:00:00Z',
                            'type': 'EventMessage'},
                           ...]}
