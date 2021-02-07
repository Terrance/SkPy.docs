Event processing
================

In order to react to incoming messages and event, an event loop is needed.

Implementing an event loop
--------------------------

The :class:`.SkypeEventLoop` class (a subclass of :class:`.Skype`) provides a base to write event processing programs.  You should implement :meth:`.SkypeEventLoop.onEvent` to add your own event handling code, and call :meth:`.SkypeEventLoop.loop` from the top level to execute your loop forever.

A complete script may look like the following::

    from getpass import getpass
    from skpy import SkypeEventLoop


    class MySkype(SkypeEventLoop):

        def onEvent(self, event):
            print(repr(event))


    if __name__ == "__main__":
        sk = MySkype("fred.2", getpass(), autoAck=True)
        sk.subscribePresence() # Only if you need contact presence events.
        sk.loop()

Note that contact presence events are not sent by default, and you must opt into them by calling :meth:`.Skype.subscribePresence` before :meth:`.SkypeEventLoop.loop`.

When ran, this will print each event as it's received::

    >>> sk = MySkype(tokenFile=".tokens-fred.2", autoAck=True)
    >>> sk.loop()
    SkypePresenceEvent(id=1000, ..., userId='joe.4', online=True)
    SkypeEndpointEvent(id=1001, ..., userId='joe.4')
    SkypePresenceEvent(id=1002, ..., userId='anna.7', online=True)
    SkypeEndpointEvent(id=1003, ..., userId='anna.7')
    SkypeEndpointEvent(id=1004, ..., userId='anna.7')
    ...

Acknowledgements
----------------

Each event can be acknowledged, to tell Skype it has been processed and avoid it being resent.  If you don't want or need to think about this, just set ``autoAck=True`` when creating your class instance.  To handle acknowledgements manually, you need to call :meth:`.SkypeEvent.ack` on each event.
