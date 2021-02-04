Working with conversations
==========================

The :attr:`.Skype.chats` field, an instance of :class:`SkypeChats`, provides a similar interface to conversations that :attr:`contacts <.Skype.contacts>` does for users.

Each conversation has a unique identifier, :attr:`.SkypeChat.id`, of the form ``<type>:<id>``.  If you know the identifier of a specific conversation, you can retrieve it with a key lookup::

    >>> sk.chats["8:joe.4"]
    SkypeSingleChat(id='8:joe.4', userId='joe.4')

.. tip::
    Single (one-to-one) conversations are usually identified with ``8:<username>``, where ``username`` is that of the other contact.  Some external protocols may use a different type number, for example ``1`` for Messenger contacts.  Group conversation identifiers looks like ``19:<random>@thread.skype``.

Each contact has a corresponding :attr:`.SkypeUser.chat` reference to their one-to-one conversation::

    >>> sk.contacts["joe.4"].chat
    SkypeSingleChat(id='8:joe.4', userId='joe.4')

Skype doesn't provide a complete list of all conversations the user is party to.  However, single or group conversations with recent activity can be retrieved with :meth:`.SkypeChats.recent`.  This can be called multiple times to fetch the next batch.  To iterate over all conversations, you might do something like this::

    >>> while True:
    ...     for chat in sk.chats.recent():
    ...         print(chat.id)
    ...     else:
    ...         # No more chats returned.
    ...         break
    ...
    8:joe.4
    8:mary.9
    19:a...@thread.skype
    19:b...@thread.skype

You can create a new group chat with a list of participants, optionally appointing some of them as group admins in addition to the connected user::

    >>> sk.chats.create(members=("joe.4", "daisy.5"), admins=("joe.4",))
    SkypeGroupChat(id='19:...@thread.skype', creatorId='fred.2', userIds=['fred.2', 'joe.4', 'daisy.5'], ...)
