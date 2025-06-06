Introduction to Qtrade
======================

This is a very basic Python 3 wrapper for the
`Questrade API <https://www.questrade.com/api/documentation/getting-started>`_, a Canadian low cost
broker.

Installation
------------

This package is available via `PyPI <https://pypi.org/project/qtrade/>`_ and can be installed via
the command

.. code:: bash

  pip install qtrade_rr

Usage
-----

The main class of the package is called ``Questrade`` and houses most of the functionality provided
by the package. Below are a few examples for possible use cases.

Token management
^^^^^^^^^^^^^^^^

The central class can be initialized via

.. code:: python

  from qtrade_rr import Questrade

  qtrade = Questrade(access_code='<access_code>')

where ``<access_code>`` is the token that one gets from the Questrade API portal. It is called
``access_code`` since this initial token is used to get the full token data that will include

.. code:: python

  {'access_token': <access_token>,
   'api_server': '<api_url>',
   'expires_in': 1234,
   'refresh_token': <refresh_token>,
   'token_type': 'Bearer'}

The first call initializes the class and the second call gets the full token.

Another way to initialize the class is to use a token yaml-file via:

.. code:: python

  qtrade = Questrade(token_yaml='<yaml_path>')

where the yaml-file would have the general form

..code:: yaml

  access_token: <access_token>
  api_server: <api_url>
  expires_in: 1234
  refresh_token: <refresh_token>
  token_type: Bearer

If the token is expired, one can use

.. code:: python

  qtrade.refresh_token()

to refresh the access token using the saved refresh token.

Once the tokens are set correctly, I have currently added methods to get ticker quotes, the
current status of all positions in any Questrade account that is associated with the tokens,
any account activities such as trades and dividend payments as well as historical data for
tickers that are supported by Questrade.

Basic functionality
^^^^^^^^^^^^^^^^^^^

There currently exists some basic functionality to get stock information via

.. code:: python

  aapl, amzn = qtrade.ticker_information(['AAPL', 'AMZN'])

and current stock quotes can be obtained via

.. code:: python

  aapl_quote, amzn_quote = qtrade.get_quote(['AAPL', 'AMZN'])

In addition, one can get historical stock quotes via

.. code:: python

  aapl_history = qtrade.get_historical_data('AAPL', '2018-08-01', '2018-08-21','OneHour')

Here, the last input parameter is the interval between quotes. Another option could be ``'OneDay'``.
For more options, see the `Questrade API description <http://www.questrade.com/api/documentation/rest-operations/enumerations/enumerations#historical-data-granularity>`_.

Account information
^^^^^^^^^^^^^^^^^^^

In addition, the Questrade API gives access to account information about the accounts connected to
the token. The accounts IDs can be accessed via

.. code:: python

  account_ids = qtrade.get_account_id()

By using the correct account ID, one can get the positions of the accounts via

.. code:: python

  positions = qtrade.get_account_positions(account_id=123456)

Finally, there exists a method to get all account activities (trades, dividends received, etc.) of
an account in a certain time frame via

.. code:: python

  activities = qtrade.get_account_activities(123456, '2018-08-01', '2018-08-16')

Contributors
------------

Contributions are always appreciated! For example:

- open an issue for a missing feature or a bug
- give feedback about existing functionality
- make suggestions for improvements
- submit a PR with a new feature (though reaching out would be appreciated)
- etc.

There is a test suite that can be run via ``python -m pytest``. This project uses ``pre-commit``
and ``black`` which takes care of automatic code formatting and linting. When setting up the development
environment, run ``pre-commit install`` to set up the hook. This will run black automatically when
committing code changes.

Disclaimer
----------

I am in no way affiliated with Questrade and using this API wrapper is licensed via the MIT license.
