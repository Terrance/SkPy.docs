Logging in
==========

SkPy provides two ways to authenticate to Skype: SOAP and Live login.

Providers
---------

SOAP login is preferred; it's a newer authentication flow that supports accounts using two-factor authentication, however it requires Skype accounts to be linked to a Microsoft account with a working Outlook.com email address.

Live authentication is a legacy flow that automates the browser login page; it's brittle and known to cause issues for some users, but it works for older accounts without a linked Microsoft account, and accepts a wider range of credentials.

=================================  ======  ======
Features                           SOAP    Live
=================================  ======  ======
**Valid usernames**
-------------------------------------------------
Email address                      ✔       ✔
Phone number                               ✔
Skype username                             ✔

**Valid passwords**
-------------------------------------------------
Account password                   ✔ [1]_  ✔
Application-specific token         ✔

**Restrictions**
-------------------------------------------------
Supports 2FA-enabled accounts      ✔
Supports accounts without MS link          ✔
=================================  ======  ======

.. [1] Only for accounts without two-factor authentication enabled.

.. seealso::
    :ref:`Authentication` -- for troubleshooting failed logins.

Default handling
----------------

A connection to Skype is made when first creating a :class:`.Skype` instance, if you provide a username and password::

    >>> from skpy import Skype
    >>> from getpass import getpass
    >>> Skype("fred.2", getpass())
    Password:
    Skype(userId='fred.2')

In this form, the best authentication method will be chosen for you (SOAP authentication for Microsoft accounts with an email address as the username, or Live/legacy authentication for accounts with Skype usernames and phone numbers).

Manual control
--------------

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
