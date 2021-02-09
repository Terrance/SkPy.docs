Working with messages
=====================

Conversation history
--------------------

Once you've obtained a conversation, you can call :meth:`.SkypeChat.getMsgs` to retrieve a batch of messages from that conversation.  This works similarly to :meth:`.SkypeChats.recent` in that you'll need to call it repeatedly for subsequent batches::

    >>> ch = sk.chats["8:joe.4"]
    >>> ch.getMsgs()
    [SkypeMsg(id='1453283895457', type='Text', time=datetime.datetime(2016, 1, 20, 9, 58, 15, 341000), ...),
     SkypeMsg(id='1452949957379', type='Text', time=datetime.datetime(2016, 1, 16, 13, 12, 37, 109000), ...), ...]

The first call will retrieve the most recent messages of the conversation, and subsequent calls will fetch older messages.  If new messages arrive in the meantime, the next call will give you any as-yet-undelivered messages first, then return to the conversation history::

    >>> # 2016-01-20 10:00:00: a new message arrives.
    >>> ch.getMsgs()
    [# New message returned first.
     SkypeMsg(id='1453284000402', ..., time=datetime.datetime(2016, 1, 20, 10, 00, 00, 84000), ...),
     # Chat history continues...
     SkypeMsg(id='1452949605841', ..., time=datetime.datetime(2016, 1, 16, 13, 6, 45, 927000), ...),
     SkypeMsg(id='1452949581027', ..., time=datetime.datetime(2016, 1, 16, 13, 6, 21, 423000), ...), ...]

Parsing message content
-----------------------

All messages have a :attr:`.SkypeMsg.content` field containing the raw HTML code received from Skype.

SkPy provides a number of helper message classes to access rich content.  You can check the type of the message with an :func:`isinstance` check against the subclasses listed under :ref:`Messages`, such as :class:`SkypeTextMsg` or :class:`SkypeFileMsg`.

Text-only messages come with the fields :attr:`.SkypeTextMsg.plain` and :attr:`.SkypeTextMsg.markup`, the former of which strips out markup tags, whilst the latter replaces them with the Markdown-like syntax used by clients::

    >>> msg
    SkypeTextMsg(..., type='RichText', content='<b...>Bold!</b>')
    >>> msg.content
    '<b...>Bold!</b>'
    >>> msg.plain
    'Bold!'
    >>> msg.markup
    '*Bold!*'

File and image messages allow you to retrieve the attached file through :attr:`.SkypeFileMsg.fileContent`::

    >>> msg
    SkypeImageMsg(..., type='RichText/UriObject', ..., file=File(name='smile.png'), ...)
    >>> import os.path
    >>> with open(os.path.join("/tmp", msg.file.name), "wb") as f:
    ...     f.write(msg.fileContent) # Write the file to disk.

Shared contacts and locations can be similarly retrieved from :attr:`.SkypeContactMsg.contacts` and :attr:`.SkypeLocationMsg.latitude` / :attr:`.SkypeLocationMsg.longitude`.

Sending messages
----------------

You can send a message using :meth:`.SkypeChat.sendMsg`::

    >>> ch = sk.chats["8:joe.4"]
    >>> msg = ch.sendMsg("Hello.")
    >>> msg
    SkypeTextMsg(..., type='Text', ..., userId='fred.2', chatId='8:joe.4', content='Hello.')
    >>> ch.sendMsg(SkypeMsg.bold("Bold!"), rich=True)
    SkypeTextMsg(..., type='RichText', ..., userId='fred.2', chatId='8:joe.4', content='<b...>Bold!</b>')

Messages are sent in plain text by default -- pass ``rich=True`` to enable parsing of Skype's HTML subset.  A number of formatting helper methods are provided on the :class:`.SkypeMsg` class.

If you need to correct the text of a sent message, you can send it again and include the message ID under the ``edit`` parameter::

    >>> ch.sendMsg("Hi!", edit=msg.id)
    SkypeTextMsg(..., type='Text', ..., content='Hi!')

To send a file rather than text, use :meth:`.SkypeChat.sendFile`::

    >>> with open("/tmp/smile.png", "rb") as f:
    ...     ch.sendFile(f, "smile.png", image=True)
    ...
    SkypeImageMsg(..., type='RichText/UriObject', ..., file=File(name='smile.png'), ...)

Set ``image=True`` to have Skype generate an image thumbnail preview in the message.

.. seealso::
    :ref:`Message formatting` -- supported HTML markup in various Skype clients.
