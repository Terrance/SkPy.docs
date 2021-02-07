Authentication
==============

Logging in with a Microsoft account is a fickle operation -- here are some of the issues you might come across.

.. seealso::
    :ref:`Logging in` -- how to handle user logins.

Error messages
--------------

You may receive one of these errors whilst trying to login.  Note that these are messages provided by the login service for a given account, and are generally not bugs in the library -- due to the nature of controlling the login page, some accounts or users see harsher responses than others, most likely dependent on the client IP address and its history of interactions.

SOAP
~~~~

wsse:FailedAuthentication - Authentication Failure
    The email address or password is incorrect.  You should use the Microsoft account's email address, not a phone number or Skype username.  If two-factor authentication is enabled, you should provide an application-specific password.

wsse:FailedAuthentication - Profile accrual is required
    Could be caused by the Microsoft account's primary alias not being an email address.  You may need to enable Outlook on the account if it was originally registered without email.

Live
~~~~

Account action required (https://...), login with a web browser first
    An interstitial page is blocking the login flow.  SkPy will not click through any authentication screens for you, so you must login in a browser to see and complete them yourself.  This may include terms of service changes, account security notices, and other prompts from Microsoft during authentication.  Some of these may only show up in the browser if you use the same IP address as where SkPy is connecting from.

Rate limiting
-------------

If you make too many authentication attempts, you may become temporarily rate limited by the Skype API, or be required to complete a captcha to continue.  For the latter, this needs to be done in a browser with a matching IP address.

To avoid this, you should reuse the Skype token where possible.  A token will usually only last for 24 hours (even ``web.skype.com`` forces re-authentication after that time), though you can check the expiry with :attr:`.SkypeConnection.tokenExpiry`.

Session expiry
--------------

When your session expires, you'll need to reauthenticate to continue.  Assuming you provided credentials at startup, these will be reused if needed and your session should automatically refresh.  Whilst token files can be written, these are usually no longer effective due to the required round-trip of the Microsoft login page, for which cookies and session data are not stored outside of an active session.
