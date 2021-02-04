Working with messages
=====================

Once you've obtained a conversation, you can call :meth:`.SkypeChat.getMsgs` to retrieve a batch of messages from that conversation.  This works similarly to :meth:`.SkypeChats.recent` in that you'll need to call it repeatedly for subsequent batches::

    >>> sk.chats["8:joe.4"].getMsgs()
    [SkypeMsg(id='1453283895457', type='Text', time=datetime.datetime(2016, 1, 20, 9, 58, 15, 341000), ...),
     SkypeMsg(id='1452949957379', type='Text', time=datetime.datetime(2016, 1, 16, 13, 12, 37, 109000), ...), ...]

The first call will retrieve the most recent messages of the conversation, and subsequent calls will fetch older messages.  If new messages arrive in the meantime, the next call will give you any as-yet-undelivered messages first, then return to the conversation history.

You can send a message using :meth:`.SkypeChat.sendMsg`::

    >>> ch = sk.chats["8:joe.4"]
    >>> ch.sendMsg("Hello.")
    SkypeMsg(..., type='Text', ..., userId='fred.2', chatId='8:joe.4', content='Hello.')
    >>> ch.sendMsg(SkypeMsg.bold("Bold!"), rich=True)
    SkypeMsg(..., type='RichText', ..., userId='fred.2', chatId='8:joe.4', content='<b...>Bold!</b>')

Messages are sent in plain text by default -- pass ``rich=True`` to enable parsing of Skype's HTML subset.  A number of formatting helper methods are provided on the :class:`.SkypeMsg` class.
