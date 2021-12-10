- [**Getting Started Guide**](#Getting-Started-Guide)

  - [**The Server**](#The-Server)

- [**Spot REST API**](#rest-api)

  - [**URL Access**](#URL-Access)
  - [**Request**](#Request)
  - [**List of REST API**](#List-of-REST-API)
  - [**Get Symbols List**](#Get-Symbols-List)
  - [**Get All Tickers**](#Get-All-Tickers)
  - [**Get Recent Fills**](#Get-Recent-Fills)
  - [**Get Ticker**](#Get-Ticker)
  - [**Get Klines**](#Get-Klines)
  - [**Get Order Book**](#Get-Order-Book)

- [**Spot Websocket API**](#websocket-api)

  - [**URL Access**](#host-url)
  - [**Request**](#Request)
  - [**Subscribe for Real-Time Trade Information**](#Subscribe-for-Real-Time-Trade-Information)
  - [**Subscribe for order book**](#Subscribe-for-order-book)
  - [**Subscribe for Klines Data**](#Subscribe-for-Klines-Data)


## Getting Started Guide

**Welcome to the User and Developer Documentation. coinmark.me offers simple and easy-to-use API Interface to provide account and order management as well as market data.**


- REST API

  Provides account and order management as well as market data query and we recommend users to use REST API to perform function e.g. account balance query, trading, order management etc.

- Websocket API

  Provides depth, real-time transaction information as well as market conditions and we recommend users to use Websocket to get real-time data.

### The Server

coinmark.me server is based in Tokyo and to minimize delays on API access, we recommend using compatible servers that can communicate smoothly with our servers in Tokyo.

<br>

## Spot REST API

### URL Access

- **[https://openapi.coinmark.me](https://www.coinmark.me)**

### Request

#### Introduction

REST API provides account and order management as well as market data query.

All requests are based on HTTPS protocol and the content-type in the request header must follow the stipulated format:

- **content-type:application/x-www-form-urlencoded**

#### Status Code

Status Code | Description                                         | Remarks
----------- | --------------------------------------------------- | -----------------------------------------------------
0           | Success                                             | code=0 Success code >0 Failed
5           | Failed to place order                               | Please check the accuracy of order price and quantity
6           | Quantity is less than minimum value                 |
7           | Quantity is more than maximum value                 |
8           | Failed to cancel order                              |
9           | Transactions are frozen                             |
13          | System error                                        |
19          | Insufficient Available Balance                      |
22          | Order does not exis                                 |
23          | Trade quantity is missing                           |
24          | Trade price is missing                              |
100001      | Abnormal System                                     |
100002      | System Upgrade                                      |
100004      | Request parameters are not legal                    |
100005      | Error in signature parameters                       |
100007      | Illegal IP                                          | Server IP is not listed in the API-Bound IP List
110004      | Withdrawals are frozen                              |
110025      | Account is being locked by the system administrator |
110041      | Frequency on URL access is too high                 |


### List of Spot REST API

API                                                                | Interface Type    | Signature | Frequency Limit | Description
------------------------------------------------------------------ | ----------------- | --------- | --------------- | ------------------------------
[GET /open/api/common/symbols](#Get-Symbols-List)                  | Public interfac   | X         | 10 times/sec    | Get Symbols List
[GET /open/api/get_allticker](#Get-All-Tickers)                    | Public interfac   | X         | 10 times/sec    | Get All Tickers
[GET /open/api/market](#Get-Recent-Fills)                          | Public interfac   | X         | 10 times/sec    | Get Recent Fills
[GET /open/api/get_ticker](#Get-Ticker)                            | Public interfac   | X         | 10 times/sec    | Get Ticker
[GET /open/api/get_trades](#Get-Trade-Histories)                   | Public interfac   | X         | 10 times/sec    | Get Trade Histories
[GET /open/api/get_records](#Get-Klines)                           | Public interfac   | X         | 10 times/sec    | Get Klines
[GET /open/api/market_dept](#Get-Order-Book)                       | Public interfac   | X         | 10 times/sec    | Get Order Book


### Get Spot Symbols List

#### GET [/open/api/common/symbols](https://openapi.coinmark.me/open/api/common/symbols)

#### Entry Parameters: No

#### Return Parameters:

Parameters       | Data Typ | Description
---------------- | -------- | ----------------------------------
code             | string   | code=0 Success， code >0 Failed
symbol           | string   | symbol
count_coin       | string   | Quote Currency
base_coin        | string   | Base Currency
amount_precision | number   | Quantity precision (0 is a number)
price_precision  | number   | Price precision (0 is a number)

#### Return to example::

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "symbol": "COINMARKusdt",
            "count_coin": "USDT",
            "amount_precision": 4,
            "base_coin": "COINMARK",
            "price_precision": 6
        },
        {
            "symbol": "vdsusdt",
            "count_coin": "USDT",
            "amount_precision": 2,
            "base_coin": "BTC",
            "price_precision": 4
        },
        ...
    ]
}
```

### Get All Spot Tickers

#### GET [/open/api/get_allticker](https://openapi.coinmark.me/open/api/get_allticker)

#### Entry Parameters: No

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success, code >0 Failed
symbol     | string    | symbol
vol        | string    | 24H Volume
high       | string    | 24H High
last       | number    | Last
low        | string    | 24H Low
buy        | number    | Buy
sell       | number    | Sell
change     | string    | 24H Price change
rose       | string    | 24H Change rate

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "ticker": [
            {
                "symbol": "COINMARKusdt",
                "high": "0.1235",
                "vol": "31753853.80270792",
                "last": 0.114906,
                "low": "0.1111",
                "buy": 0.114887,
                "sell": 0.114967,
                "change": "0.0085224",
                "rose": "0.0085224"
            },
            {
                "symbol": "vdsusdt",
                "high": "3.39",
                "vol": "532061.01067007",
                "last": 3.1459,
                "low": "3.1",
                "buy": 3.14,
                "sell": 3.1541,
                "change": "-0.00427296",
                "rose": "-0.00427296"
            },
            {
                "symbol": "btcusdt",
                "high": "10716.3335",
                "vol": "20433.12745191",
                "last": 10521.9785,
                "low": "9864.9351",
                "buy": 10515.7454,
                "sell": 10527.1895,
                "change": "-0.00423288",
                "rose": "-0.00423288"
            },
            ...
        ],
        "date": 1563207200947
    }
}
```

### Get Recent Fills

#### GET [/open/api/market](https://openapi.coinmark.me/open/api/market)

#### Entry Parameters: No

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | List Recent Fills

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "eosbtc": 0.00038891,
        "attusdt": 0.000041,
        "heyusdt": 0.021607,
        "ontbtc": 0.00008929,
    ...
    }
}
```

### Get Ticker

#### GET [/open/api/get_ticker](https://openapi.coinmark.me/open/api/get_ticker?symbol=btcusdt)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description | Value Range
---------- | --------- | --------- | ----------- | -----------------------------
symbol     | true      | string    | symbol      | btcusdt, ltcusdt, ethusdt ...

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
symbol     | string    | symbol
vol        | string    | Volume
high       | string    | High
last       | number    | Last
low        | string    | Low
buy        | number    | Buy
sell       | number    | Sell
change     | string    | Price change
rose       | string    | Change rate

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "high": "10753.6563",
        "vol": "20193.35399854",
        "last": 10335.8936,
        "low": "9287.7207",
        "buy": 10328.6517,
        "sell": 10340.1291,
        "rose": "-0.00963801",
        "time": 1563530414000
    }
}
```

### Get Trade Histories

#### GET [/open/api/get_trades](https://openapi.coinmark.me/open/api/get_trades?symbol=btcusdt)

#### Entry Parameters::

Parameters | Necessary | Data Type | Description | Value Range
---------- | --------- | --------- | ----------- | -----------------------------
symbol     | true      | string    | Symbol      | btcusdt, ltcusdt, ethusdt ...

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | Trade Record

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "amount": 0.55,               // Trade volume
            "price": 0.18519949,          // Trade price
            "id": 447121,
            "type": "buy"                 // type，buy or sell
            "ts":1553690617000            // timestamp
            "ds":2019-3-27 20:43:37       // Trade Time Format Display
        },
        ...
    ]
}
```

### Get Klines

#### GET [/open/api/get_records](https://openapi.coinmark.me/open/api/get_records?symbol=btcusdt&period=1)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                                                                   | Value Range
---------- | --------- | --------- | --------------------------------------------------------------------------------------------- | -----------------------------
symbol     | true      | string    | symbol                                                                                        | btcusdt, ltcusdt, ethusdt ...
period     | true      | number    | K Line Cycle in Minutes, 1 for 1 minute, 1 Day for 1440 minutes 1/5/15/30/60/1440/10080/43200

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
data       | object    | Kline Data

##### data description:

```python
"data": [
  [
    1558586460,   // Kline opening timestamp
    7654.7866,    // Opening price
    7654.7866,    // High
    7654.0322,    // Low
    7654.0322,    // Closing price
    26.9234       // Trade volume
  ],
  ...
]
```

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": [
        [
            1558586460,
            7654.7866,
            7654.7866,
            7654.0322,
            7654.0322,
            26.9234
        ],
        [
            1558586520,
            7654.0322,
            7654.0322,
            7654.0322,
            7654.0322,
            0.0
        ],
        ...
    ]
}
```

### Get Order Book

#### GET [/open/api/market_dept](https://openapi.coinmark.me/open/api/market_dept?symbol=btcusdt&type=step0)

#### Entry Parameters:

Parameters | Necessary | Data Type | Description                                            | Value Range
---------- | --------- | --------- | ------------------------------------------------------ | -----------------------------
symbol     | true      | string    | symbol                                                 | btcusdt, ltcusdt, ethusdt ...
type       | true      | string    | Depth Type (Combined Depth 0-2) Step0 as Highest Depth | step0/step1/step2

#### Return Parameters:

Parameters | Data Type | Description
---------- | --------- | ------------------------------
code       | string    | code=0 Success， code >0 Failed
tick       | object    | Order Book

##### description :

```python
"tick": {
    "asks": [             // Sell
        [
            10352.1109,
            0.1959
        ],
        [
            10352.1315,
            0.2393
        ],
        ...
    ],
    "bids": [             // Buy
        [
            10336.1313,
            0.8707
        ],
        [
            10334.3287,
            0.1721
        ],
        ...
    ],
    "time": null
}
```

#### Return to example:

```python
{
    "code": "0",
    "msg": "suc",
    "data": {
        "tick": {
            "asks": [
                [
                    10352.1109,
                    0.1959
                ],
                [
                    10352.1315,
                    0.2393
                ],
                ...
            ],
            "bids": [
                [
                    10336.1313,
                    0.8707
                ],
                [
                    10334.3287,
                    0.1721
                ],
                ...
            ],
            "time": null
        }
    }
}
```

## Spot Websocket API

### URL Access URL

#### [wss://ws.coinmark.me/kline-api/ws]((https://www.coinmark.me)) [Recommend]

#### [wss://ws.COINMARKcoin.pro/kline-api/ws](https://www.coinmark.me)

### Connection Request

#### Compressed Data

- All data returned by WebSocket API are compressed by GZIP so clients are required to unzip upon receipt of the data

#### Heartbeat Message

- When user's Websocket is connected to the Websocket Server, the Server will send periodic ping messages with a string of numbers as follows： {"ping": 156200607000}

- When user's Websocket receive such heartbeat message, please return a pong message with the same string of numbers： {"pong": 156200607000}

> If the Websocket Server does not receive any `pong` message after sending many `ping` messages, the Server will automatically disconnects from the user's Websocket

### Subscribe for Real-Time Trade Information

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_trade_ticker",
        "cb_id": "Customer"
    }
}
```

> Where $base$quote for trading information其中$base$quote<br>
> channel example "channel": "market_btcusdt_trade_ticker"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_trade_ticker",
    "cb_id": "Customer",
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new trading information available, the Server will auto send push messages as follows:

```python
{
    "channel":"market_$base$quote_trade_ticker", // Subscribed trading information channel
    "ts":1506584998239,                         // Request time
    "tick":{
        "id":12121,                             // the largest order ID in the data
        "ts":1506584998239,                     // the maximum time in the data
        "data":[
            {
                "id":12121,                     // trade ID
                "side":"buy",                   // Buy/Sell Direction
                "price":32.233,                 // Price
                "vol":232,                      // Volume
                "amount":323,                   // Total Amount
                "ts":1506584998239,             // Data create time
                "ds":'2017-09-10 23:12:21'
            },
            {
                "id":12120,                     // trade ID
                "side":"buy",                   // Buy/Sell Direction
                "price":32.233,                 // Price
                "vol":232,                      // Volume
                "amount":323,                   // Total Amount
                "ts":1506584998239,             // Data create time
                "ds":'2017-09-10 23:12:21'
            }
        ]
    }
}
```

### Subscribe for Depth Data

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_depth_step[0-2]",
        "cb_id": "Customer",
        "asks": 150,
        "bids": 150
    }
}
```

> $base$quote represents btcusdt and other trade pairs,step[0-2] represents depth with 3 dimension 0、1、2<br>
> channel example "channel": "market_btcusdt_depth_step0"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_depth_step[0-2]",
    "cb_id": "Customer",
    "asks": 150,
    "bids": 150,
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new information on depth data, the Server will auto send push messages:

```python
{
    "channel": "market_$base$quote_depth_step[0-2]",
    "ts": 1506584998239,
    "tick": {
        "asks": [
            [22112.22,0.9332],      // Price,Volume
            [22112.21,0.2]
        ],
        "buys": [
            [22111.22,0.9332],
            [22111.21,0.2]
        ]
    }
}
```

### Subscribe for Klines Date

#### Send a subscription message upon successful connection:

```python
{
    "event": "sub",
    "params": {
        "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
        "cb_id": "Customer"
    }
}
```

> channel example "channel": "market_btcusdt_kline_1min"

#### Return to successful subscription message

```python
{
    "event_rep": "subed",
    "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
    "cb_id": "Customer",
    "ts": 1506584998239,
    "status": "ok"
}
```

#### Whenever there is new information on Kline data, the Server will auto send push messages as follows:

```python
{
    "channel": "market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
    "ts": 1506584998239,
    "tick": {
        "id": 1506602880,
        "amount": 123.1221,
        "vol": 1212.12211,
        "open": 2233.22,
        "close": 1221.11,
        "high": 22322.22,
        "low": 2321.22
    }
}
```

[^_^]: aadf4
