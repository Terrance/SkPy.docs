Working with contacts
=====================

Each :class:`.Skype` instance has a :attr:`contacts <.Skype.contacts>` field, an instance of :class:`.SkypeContacts`.

Finding specific users
----------------------

To find a specific contact or user, use key lookups with user identifiers::

    >>> sk = Skype(...)
    >>> sk.contacts
    SkypeContacts()
    >>> sk.contacts["joe.4"] # Joe is a contact of Fred's.
    SkypeContact(id='joe.4', name=Name(first='Joe', last='Bloggs'), ..., authorised=True, blocked=False)
    >>> sk.contacts["anna.7"] # Here, Anna is not a contact.
    SkypeUser(id='anna.7', name=Name(first='Anna', last='Cooper'), ...)

Note also the special :attr:`.Skype.user` field, a contact object for the connected account::

    >>> sk.user # It's you!
    SkypeContact(id='fred.2', name=Name(first='Fred', last='Adams'), ...)
    >>> sk.contacts["fred.2"] is sk.user
    True

Generally, you will get less information out of :class:`.SkypeUser` objects as they only access public info.  :class:`.SkypeContact` objects will be provided when applicable.

Iterating contacts
------------------

You can also iterate over :class:`.SkypeContacts` in order to loop through all contacts of the connected account::

    >>> for contact in sk.contacts:
    ...     print(contact.id)
    ...
    joe.4
    daisy.5

Contact requests
----------------

Incoming contact requests can be obtained through :meth:`.SkypeContacts.requests`, which returns a list of requests.  You can call one of :meth:`.SkypeRequest.accept` and :meth:`.SkypeRequest.reject` to act on a given request.

For example, to automatically accept requests::

    >>> for request in sk.contacts.requests():
    ...     request.accept()
