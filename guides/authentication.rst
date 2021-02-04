Authentication
==============

A connection to Skype is made when first creating a :class:`.Skype` instance::

    >>> from skpy import Skype
    >>> from getpass import getpass
    >>> Skype("fred.2", getpass())
    Password:
    Skype(userId='fred.2')

In this form, the best authentication method will be chosen for you (SOAP authentication for Microsoft accounts with an email address as the username, or Live/legacy authentication for accounts with Skype usernames and phone numbers).

.. warning::
    If you make too many authentication attempts, you may become temporarily rate limited by the Skype API, or be required to complete a captcha to continue.  For the latter, this needs to be done in a browser with a matching IP address.

    To avoid this, you should reuse the Skype token where possible.  A token will usually only last for 24 hours (even ``web.skype.com`` forces re-authentication after that time), though you can check the expiry with :attr:`.SkypeConnection.tokenExpiry`.

Depending on the interface being provided by the user, it may not be desirable to require credentials when instantiating the :class:`.Skype` class.  In this case, pass no arguments to produce an as-yet-unconnected instance::

    >>> sk = Skype()
    >>> sk.conn
    SkypeConnection(connected=False)

After you've obtained credentials from the user, the Skype connection object :attr:`.Skype.conn` can be manipulated directly, to set a username/password pair and the desired authentication mechanism, or to sign in as a guest::

    >>> # Auto-detect the best authentication method:
    >>> sk.conn.setUserPwd("fred.2", getpass())
    Password:
    >>> sk.getSkypeToken()
    >>> sk.conn
    SkypeConnection(userId='fred.2', connected=True, guest=False)

    >>> # Or use a Microsoft account, including 2FA-protected accounts:
    >>> sk.conn.soapLogin("fred.adams@outlook.com", getpass())
    Password:
    >>> sk.conn
    SkypeConnection(userId='fred.2', connected=True, guest=False)

    >>> # Or use legacy authentication with a Skype username or phone number:
    >>> sk.conn.liveLogin("fred.2", getpass())
    Password:
    >>> sk.conn
    SkypeConnection(userId='fred.2', connected=True, guest=False)

    >>> # Or join a conversation as a guest:
    >>> sk.conn.guestLogin("https://join.skype.com/abcdefghijkl", "Fred")
    >>> sk.conn
    SkypeConnection(userId='guest:a1b2c3d4...', connected=True, guest=True)
