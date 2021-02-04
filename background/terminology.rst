Terminology
===========

Connected account
    user currently authenticated to the API
User identifier
    user's Skype username, e.g. ``fred.2``
Prefix
    conversation type:
        | ``1`` = Messenger user
        | ``2`` = Skype for Business user
        | ``8`` = Skype user
        | ``19`` = group
        | ``28`` = agent/bot account (e.g. ``28:concierge``)
Chat identifier
    unique chat name, e.g. ``a1b2c3d4...@thread.skype``
Thread identifier
    combination of prefix and user/chat identifier, e.g. ``8:fred.2``, ``19:a1b2c3d4...@thread.skype``
Blob
    old-style group chat identifier (and only identifier for P2P chats)

Sample users
------------

Throughout this documentation, the following users are mentioned:

- **Fred Adams**, ``fred.2`` -- the authenticated user
- **Joe Bloggs**, ``joe.4`` -- a contact of Fred's
- **Anna Cooper**, ``anna.7`` -- not a contact of Fred's
