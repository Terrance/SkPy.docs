Static resources
----------------

.. http:get:: https://static-asm.secure.skypeassets.com/pes/v1/configs/(string:hash)/views/en

    Retrieve a list of all emoticons and videos, along with shortcuts and visibility.

    The hash changes each time the set of resources is changed.  SkPy currently comes bundled with emoticons from hash ``21280e53cdb24cde94cf4d4d0f5cb7c7`` (shortly after Christmas emoticons were added).

    :param hash: version identifier for the static set
