Installation
============

From PyPI
---------

The easiest way to get started is by installing from `PyPI <https://pypi.org/project/SkPy>`_::

    $ pip install skpy

From the repo
-------------

SkPy can also be installed straight from the repo.  First, clone a copy of it locally::

    $ git clone git@github.com:Terrance/SkPy.git
    $ cd SkPy

Then install SkPy as a link to the checkout::

    $ pip install -e .

You can alternatively fetch the requirements and run the setup script manually::

    $ pip install -r requirements.txt
    $ python setup.py install

You can also just go ahead and import :mod:`skpy` from the repo root, assuming the requirements are installed.

Requirements
------------

- Python 2.6+ (includes 3.x)
- `BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_
- `Requests <http://www.python-requests.org/en/latest/>`_ [1]_
- `Responses <https://github.com/getsentry/responses>`_ (for tests)

.. [1] Note that Requests no longer supports Python 3.2 -- the last working version is 2.10.0.
