================================
Welcome to python-binance v0.6.9
================================

.. image:: https://img.shields.io/pypi/v/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/l/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/travis/sammchardy/python-binance.svg
    :target: https://travis-ci.org/sammchardy/python-binance

.. image:: https://img.shields.io/coveralls/sammchardy/python-binance.svg
    :target: https://coveralls.io/github/sammchardy/python-binance

.. image:: https://img.shields.io/pypi/wheel/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/pyversions/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

This is an unofficial Python wrapper for the `Binance exchange REST API v1/3 <https://github.com/binance-exchange/binance-official-api-docs>`_. I am in no way affiliated with Binance, use at your own risk.

If you came here looking for the `Binance exchange <https://www.binance.com/?ref=10099792>`_ to purchase cryptocurrencies, then `go here <https://www.binance.com/?ref=10099792>`_. If you want to automate interactions with Binance stick around.

Source code
  https://github.com/sammchardy/python-binance

Documentation
  https://python-binance.readthedocs.io/en/latest/

Binance API Telegram
  https://t.me/binance_api_english

Blog with examples
  https://sammchardy.github.io

Make sure you update often and check the `Changelog <https://python-binance.readthedocs.io/en/latest/changelog.html>`_ for new features and bug fixes.

Features
--------

- Implementation of all General, Market Data and Account endpoints.
- Simple handling of authentication
- No need to generate timestamps yourself, the wrapper does it for you
- Response exception handling
- Websocket handling with reconnection and multiplexed connections
- Symbol Depth Cache
- Historical Kline/Candle fetching function
- Withdraw functionality
- Deposit addresses

Quick Start
-----------

`Register an account with Binance <https://www.binance.com/register.html?ref=10099792>`_.

`Generate an API Key <https://www.binance.com/userCenter/createApi.html>`_ and assign relevant permissions.

.. code:: bash

    pip install python-binance


.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

    # get market depth
    depth = client.get_order_book(symbol='BNBBTC')

    # place a test market buy order, to place an actual order use the create_order function
    order = client.create_test_order(
        symbol='BNBBTC',
        side=Client.SIDE_BUY,
        type=Client.ORDER_TYPE_MARKET,
        quantity=100)

    # get all symbol prices
    prices = client.get_all_tickers()

    # withdraw 100 ETH
    # check docs for assumptions around withdrawals
    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        result = client.withdraw(
            asset='ETH',
            address='<eth_address>',
            amount=100)
    except BinanceAPIException as e:
        print(e)
    except BinanceWithdrawException as e:
        print(e)
    else:
        print("Success")

    # fetch list of withdrawals
    withdraws = client.get_withdraw_history()

    # fetch list of ETH withdrawals
    eth_withdraws = client.get_withdraw_history(asset='ETH')

    # get a deposit address for BTC
    address = client.get_deposit_address(asset='BTC')

    # start aggregated trade websocket for BNBBTC
    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # do something

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    bm.start_aggtrade_socket('BNBBTC', process_message)
    bm.start()

    # get historical kline data from any date range

    # fetch 1 minute klines for the last day up until now
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # fetch 30 minute klines for the last month of 2017
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # fetch weekly klines since it listed
    klines = client.get_historical_klines("NEOBTC", Client.KLINE_INTERVAL_1WEEK, "1 Jan, 2017")

For more `check out the documentation <https://python-binance.readthedocs.io/en/latest/>`_.

Donate
------

If this library helped you out feel free to donate.

- ETH: 0x0a5EFE47E01748B0A49e14C4150a4E44eB32798f
- LTC: LhtpZ11WP5AMS4ZJYL5W6jLeFy5mgqhCbq
- BCH: 3KnvDG8McvtFhnQmwfADFApmPpR6XUDV4j
- BTC: 1FSVCvAobDBRFjS2C3S42XjXFMqBs4EBBp

Other Exchanges
---------------

If you use `Quoinex <https://accounts.quoinex.com/sign-up?affiliate=PAxghztC67615>`_
or `Qryptos <https://accounts.qryptos.com/sign-up?affiliate=PAxghztC67615>`_ check out my `python-quoine <https://github.com/sammchardy/python-quoine>`_ library.

If you use `Kucoin <https://www.kucoin.com/#/?r=E42cWB>`_ check out my `python-kucoin <https://github.com/sammchardy/python-kucoin>`_ library.

If you use `Allcoin <https://www.allcoin.com/Account/RegisterByPhoneNumber/?InviteCode=MTQ2OTk4MDgwMDEzNDczMQ==>`_ check out my `python-allucoin <https://github.com/sammchardy/python-allcoin>`_ library.

If you use `IDEX <https://idex.market>`_ check out my `python-idex <https://github.com/sammchardy/python-idex>`_ library.

If you use `BigONE <https://big.one>`_ check out my `python-bigone <https://github.com/sammchardy/python-bigone>`_ library.

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance?pixel&useReferer
