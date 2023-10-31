- [Websocket API](#websocket-api)
- [Rest API for P2B](#rest-api-for-p2b)
  - [API Information](#api-information)
  - [API Endpoints](#api-endpoints)
    - [Public](#public)
      - [Markets](#markets)
      - [Market](#market)
      - [Tickers](#tickers)
      - [Ticker](#ticker)
      - [Order book](#order-book)
      - [History](#history)
      - [Depth Result](#depth-result)
      - [Kline](#kline)
    - [Protected](#protected)
      - [Account/Balances](#accountbalances)
        - [All Balances](#all-balances)
        - [Balance by currency](#balance-by-currency)
      - [Account/Trade](#accounttrade)
        - [Create order](#create-order)
        - [Cancel order](#cancel-order)
        - [Open orders](#open-orders)
        - [Orders history by market](#orders-history-by-market)
        - [Deals history by market](#deals-history-by-market)
        - [Deals by order ID](#deals-by-order-id)
        - [Order history](#order-history)
        - [Executed history](#executed-history)

  

# Websocket API

- [WSS](https://github.com/P2B-team/P2B-WSS-Public/blob/main/wss_documentation.md)

# Rest API for P2B


## API Information

* Base URL for requests is <https://api.p2pb2b.com>

## API Endpoints

Possible errors when executing queries: 

* [Errors](./errors.md)

### Public

Public folder requests available without authentication.

**General requirements**

* All requests to a public API are executed by the GET method. With or without parameters passed to the URL.
* Parameters may be required or optional.
* More information can be found in the description of a specific endpoint.

**Info**

* **offset**  Default value = 0. The passed parameter will be selected according to these requirements. The nearest larger value will be set, which is a multiple of the change step.

* **limit**  Value change step = 1. The passed parameter will be casted to these requirements. The nearest larger value will be set, which is a multiple of the change step, but not bigger than the allowed maximum.

#### Markets


Get info on all markets.

```
GET /api/v2/public/markets
```

**Parameters:**

* Without parameters

**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/markets"
```


**Response example:**
```json
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": [
        {
            "name": "ETH_BTC",
            "stock": "ETH",
            "money": "BTC",
            "precision": {
                "money": "6",
                "stock": "4",
                "fee": "4"
            },
            "limits": {
                "min_amount": "0.001",
                "max_amount": "100000",
                "step_size": "0.0001",
                "min_price": "0.00001",
                "max_price": "922327",
                "tick_size": "0.00001",
                "min_total": "0.0001"
            }
        },
        {
            "name": "BTC_USDT",
            "stock": "BTC",
            "money": "USDT",
            "precision": {
                "money": "2",
                "stock": "6",
                "fee": "4"
            },
            "limits": {
                "min_amount": "0.00001",
                "max_amount": "9000",
                "step_size": "0.00001",
                "min_price": "0.01",
                "max_price": "1000000",
                "tick_size": "0.01",
                "min_total": "5"
            }
        }
    ],
    "cache_time": 1698731482.965841,
    "current_time": 1698731498.885766
}
```
#### Market

Get limits for market.

**Limits define trading rules on a market:**

*  If the limit is equal 0, the limit does not apply to this market.

* **step_size** - defines the intervals by which it is possible to change (increase / decrease) the amount in the range from **min_amount** to **max_amount**. The 'amount % step_size == 0' condition should be true

* **tick_size** - defines the intervals by which it is possible to change (increase / decrease) the price in the range from **min_price** to **max_price**. The 'price % tick_size == 0' condition should be true

* **min_total** - minimum allowed order total.


```
GET /api/v2/public/market
```

**Parameters:**

Name | Type | Mandatory | Example | Description
------------ | ------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.

**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/market?market=ETH_BTC"
```


**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "name":"ETH_BTC",           // market name
        "stock":"ETH",              // stock ticker
        "money":"BTC",              // money ticker
        "precision": {  
            "money":"6",            // money precision
            "stock":"3",            // stock precision
            "fee":"4"               // fee precision
        },
        "limits": {
            "min_amount":"0.001",   // min amount value
            "max_amount":"100000",  // max amount value
            "step_size":"0.001",    // min step size for changing the amount
            "min_price":"0.000001", // min price value
            "max_price":"100000",   // max price value
            "tick_size":"0.000001", // min step size for changing the price
            "min_total":"0.0001"    // min total value
        }
    }
}
```

#### Tickers

Get trade details for all tickers.

```
GET /api/v2/public/tickers
```

**Parameters:**

Without parameters

**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/tickers"
```


**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "ETH_BTC": {
            "at": 1698732273,
            "ticker": {
                "bid": "0.052422",          // best bid price
                "ask": "0.05266",           // best ask price
                "low": "0.05201",           // lowest price
                "high": "0.053",            // highest price
                "last": "0.05253",          // last price
                "vol": "227.0479",          // 24-hour trading volume denoted in stock
                "deal": "11.9195756492",    // 24-hour trading volume denoted in money
                "change": "0.98"            // 24-hour perice change
            }
        },       
        "BTC_USDT": {
            "at": 1698732273,
            "ticker": {
                "bid": "34280.08",
                "ask": "34280.28",
                "low": "34078.64",
                "high": "34855.97",
                "last": "34280.18",
                "vol": "5477.739225",
                "deal": "188772192.05708605",   
                "change": "0.05"
            }
        }
    },
    "cache_time": 1698732273.325387,
    "current_time": 1698732282.466824
}
```

#### Ticker

Get trade details for a ticker.

```
GET /api/v2/public/ticker
```

**Parameters:**

Name | Type | Mandatory | Example | Description
------------ | ------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name


**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/ticker?market=ETH_BTC"
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "bid": "34301.89",            // best bid price
        "ask": "34302.09",            // best ask price
        "open": "34240.35",           // open price
        "high": "34855.97",           // highest price
        "low": "34078.64",            // lowest price
        "last": "34302",              // last price
        "volume": "5473.04784",       // 24-hour trading volume denoted in stock
        "deal": "188611751.32669417", // 24-hour trading volume denoted in money
        "change": "0.18"              // 24-hour perice change
    },
    "cache_time": 1698732454.943941,
    "current_time": 1698732461.924536
}
```

#### Order book

Get all active orders by market.

```
GET /api/v2/public/book
```

**Parameters:**

Name | Type | Mandatory | Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name
side | STRING | YES | buy | Type of order. Valid values: 'sell, 'buy' 
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.   
limit | INT | NO | 1 | limit . Min value 1.  Default value 50 . Max value 100.  



**Сaching:**

Results are cached for ~1s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/book?market=ETH_BTC&side=sell&offset=0&limit=100"
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "offset": 0,
        "limit": 2,
        "total": 866,
        "orders": [
            {
                "id": 172449556903,               // Order id
                "left": "0.0076",                 // Left amount
                "market": "BTC_USDT",             // Market name
                "amount": "0.0076",               // Original amount 
                "type": "limit",                  // Order type
                "price": "34334.1",               // Price
                "timestamp": 1698731838.911475,   // Order creation time
                "side": "buy",                    // Order side
                "dealStock": "0",                 // Filled amount
                "dealMoney": "0"                  // Filled total
            },
            {
                "id": 172449558214,
                "left": "0.01742",
                "market": "BTC_USDT",
                "amount": "0.01742",
                "type": "limit",
                "price": "34334.1",
                "timestamp": 1698732039.467471,
                "side": "buy",
                "dealStock": "0",
                "dealMoney": "0"
            }
        ]
    },
    "cache_time": 1698732840.356483,
    "current_time": 1698732840.356609
}
```

#### History

Recent trade list by market. For each market available last 10 000 deals. 

```
GET /api/v2/public/history
```

**Parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name
lastId | INT | YES | 7460622597 | Order id
limit | INT | NO | 1 |  Limit . Min value 1.  Default value 50 . Max value 100.  

**Сaching:**

Results are cached for ~5s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/history?market=ETH_BTC&lastId=7460622597&limit=100"
```

**Response example:**
```json5
{
  "success": true,
  "errorCode": "",
  "message": "",
  "result": [
    {
      "id": 7460623849,             // Deal id
      "type": "sell",               // Deal side
      "time": 1698733296.591153,    // Deal execution time
      "amount": "0.07158",          // Deal amount
      "price": "1803.41"            // Deal price
    },
    {
      "id": 7460623833,
      "type": "sell",
      "time": 1698733294.93488,
      "amount": "0.33258",
      "price": "1803.41"
    }
  ],
  "cache_time": 1698733298.301596,
  "current_time": 1698733298.301753
}
```

#### Depth Result

Order depth for a market.

```
GET /api/v2/public/depth/result
```

**Parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.
limit | INT | NO | 1 |  Limit . Min value 1.  Default value 500 . Max value 1000.  
interval | ENUM  | NO | 0 |   Valid values: 0, 0.00000001, 0.0000001, 0.000001, 0.00001, 0.0001, 0.001, 0.01, 0.1, 1. Default 0;  
 

**Сaching:**

Results are cached for ~1s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/depth/result?market=ETH_BTC&limit=100"
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "asks": [
            [
                "4.53",     // Price
                "523.95"    // Amount
            ],
            [
                "4.54",
                "1237.64"
            ]
        ],
        "bids": [
            [
                "4.51",
                "244.75"
            ],
            [
                "4.5",
                "885.59"
            ]
        ]
    },
    "cache_time": 1698733470.469175,
    "current_time": 1698733470.469274
}
```

#### Kline

```
GET /api/v2/public/market/kline
```

Kline/candlestick bars for a market.

**Parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.
interval | ENUM | YES | 1m |  Name of the interval from the list of valid intervals
offset | INT | NO | 0 | Default value = 0. With this value the last candles are returned. the number of candles returned is determined in the 'limit'.
limit | INT | NO | 50 | Default value = 50. Value change step = 50. Max value = 500. The passed parameter will be cast to these requirements. The nearest larger value will be set, which is a multiple of the change step, but no more than the maximum allowable.



**intervals:**

m -> minutes; h -> hours; d -> days;

* 1m
* 1h
* 1d

**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/market/kline?market=ETH_BTC&interval=1m&offset=0&limit=100"
```

**Response example:**

```json5
{
  "success": true,
  "errorCode": "",
  "message": "",
  "result": [
    [
      1698732000,            // Kline open time
      "34282.21",            // Open price
      "34285.85",            // Close price
      "34352.25",            // Highest price
      "34257.93",            // Lowest price
      "81.299292",           // Volume for stock currency
      "2789867.11506117",    // Volume for money currency
      "BTC_USDT"             // Market name
    ]
  ],
  "cache_time": 1698733648.564578,
  "current_time": 1698733648.564722
}
```


### Protected

For requests in Protected folder you should provide apiKey and apiSecret. Use this step-by-step list:

* Activate 2FA in your P2B account in Account>Security <https://p2pb2b.com/account/security>
* Generate API key and secret in Account>API <https://p2pb2b.com/account/api>

**General requirements**

* A request to a private API must have a set of required headers, with which the user is authenticated.

**HEADERS:**

Name| Data | Mandatory |  Description
------------ |------------ | ------------ | ------------ 
Content-Type | application/json | YES |   Request body transferred in JSON format.
X-TXC-APIKEY | {{apiKey}} | YES | Account 'API key' from <https://p2pb2b.com/account/api>
X-TXC-PAYLOAD | {{payload}} | YES |  Body json encoded in base64
X-TXC-SIGNATURE | {{signature}} | YES |  'Payload'  encrypted using HMAC with SHA512 algorithm and 'API secret' from <https://p2pb2b.com/account/api>


* All requests to the private API are executed by the post method.
* Parameters are passed to the request body as json. A request to a private API must have the required parameters that are passed in the body of each request.

**Mandatory parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
request | STRING | YES | /api/v2/account/balances | Request endpoint
nonce | INT | YES | 1575537501932 | Timestamp in millisecond 

**nonce** - integer, preferably timestamp in milliseconds. Repeated nonce within the last 1 second are not allowed.

* Each endpoint may also have additional parameters. More information can be found in the description of a specific endpoint.

* The number of user requests to the endpoints of the protected API is limited. Not more than 10 requests per second.

#### Account/Balances

##### All Balances

List of user balances for all currencies.

```
POST /api/v2/account/balances
```

**Parameters:**

* Basic parameters should be added to the request body.


**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/account/balances' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
	"request":"/api/v2/account/balances",
	"nonce":"1698508469"
}'
```

**Response example:**
```json5
{
  "success": true,
  "errorCode": "",
  "message": "",
  "result": {
      "USDT": {
        "available": "71.81328046",
        "freeze": "10.46103091"
      },
      "BTC": {
        "available": "0.00135674",
        "freeze": "0.00020003"
      }
  }
}
```

##### Balance by currency

User balance for the selected currency.

```
POST /api/v2/account/balance
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
currency | STRING | YES | ETH | Currency from the list of existing included currencies

**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/account/balance' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
	"currency":"USDT",
	"request":"/api/v2/account/balance",
	"nonce":"1698508381"
}'
```


**Response example:**
```json5
{
  "success": true,
  "errorCode": "",
  "message": "",
  "result": {
      "available": "71.81328046",
      "freeze": "10.46103091"
  }
}
```

#### Account/Trade

##### Create order

Create in a new limit order


```
POST /api/v2/order/new
```

**Parameters:**

* Basic parameters should be added to the request body.


Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.
side | STRING | YES | buy | Valid values: 'sell, 'buy'.
amount | STRING | YES | 0.0277 | Amount.
price | STRING | YES | 0.04348 | Price.

* Orders that do not pass market limits will not be created. 
* Limits are indicated in the [market](#market), for more detail see description. 

* **amount**,**price** - must contain a numeric string.

**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/order/new' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
	"market":"ETH_BTC",
	"side":"buy",
	"amount":"0.0277",
	"price":"0.04348",
	"request":"/api/v2/order/new",
	"nonce":"1698484860"
}'
```


**Response example:**

```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "orderId": 171906478744,          // Order id
        "market": "ETH_BTC",              // Market name
        "price": "0.04348",               // Price 
        "side": "buy",                    // Side
        "type": "limit",                  // Order type
        "timestamp": 1698484861.746517,   // Order creation time
        "dealMoney": "0",                 // Filled total
        "dealStock": "0",                 // Filled amount 
        "amount": "0.0277",               // Original amount
        "takerFee": "0.002",              // taker fee
        "makerFee": "0.002",              // maker fee
        "left": "0.0277",                 // Unfilled amount
        "dealFee": "0"                    // Filled fee
    }
}
```


##### Cancel order

Cancel an active order.

```
POST /api/v2/order/cancel
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.
orderId | INT | YES | 171906478744 | Order id for cancel.


**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/order/cancel' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
    "market": "ETH_BTC",
    "orderId": "171906478744",
    "request": "/api/v2/order/cancel",
    "nonce": "1698485008"
}'  
```

**Response example:**

```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "orderId": 171906478744,
        "market": "ETH_BTC",
        "price": "0.04348",
        "side": "buy",
        "type": "limit",
        "timestamp": 1698484861.746517,
        "dealMoney": "0",
        "dealStock": "0",
        "amount": "0.0277",
        "takerFee": "0.002",
        "makerFee": "0.002",
        "left": "0.0277",
        "dealFee": "0"
    }
}
```


##### Open orders

Query unfilled or partially filled orders.

```
POST /api/v2/orders
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name.
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.
limit | INT | NO | 1 | limit . Min value 1.  Default value 50 . Max value 100.

**Request example:**

``` 
curl --location 'https://api.p2pb2b.com/api/v2/orders' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
	"market":"ETH_BTC",
	"limit":"100",
	"offset":"0",
	"request":"/api/v2/orders",
	"nonce":"1698487841"
}'
```

**Response example:**

```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": [
        {
            "orderId": 171913325964,
            "market": "ETH_BTC",
            "price": "0.06534",
            "side": "sell",
            "type": "limit",
            "timestamp": 1698487986.836821,
            "dealMoney": "0",
            "dealStock": "0",
            "amount": "0.0018",
            "takerFee": "0.0018",
            "makerFee": "0.0016",
            "left": "0.0018",
            "dealFee": "0"
        },
        {
            "orderId": 171913295975,
            "market": "ETH_BTC",
            "price": "0.04871",
            "side": "buy",
            "type": "limit",
            "timestamp": 1698487973.830216,
            "dealMoney": "0",
            "dealStock": "0",
            "amount": "0.0041",
            "takerFee": "0.0018",
            "makerFee": "0.0016",
            "left": "0.0041",
            "dealFee": "0"
        }
    ]
}
```


##### Orders history by market

Query orders history. Note, only filled or partially filled and canceled orders are returning.
Only the transaction records in the past 3 month can be queried.

```
POST /api/v2/account/market_order_history
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | BTC_USDT | Market name.
startTime | LONG | YES | 1698192000 | 
endTime | LONG | YES | 1698278400 | 
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.
limit | INT | NO | 100 | limit . Min value 1.  Default value 50 . Max value 100.

* The time between startTime and endTime can't be longer than 24 hours (86400 seconds).

**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/account/market_order_history' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
    "market": "BTC_USDT",
    "startTime": 1698192000,
    "endTime": 1698278400,
    "limit": 100,
    "offset": 0,
    "request": "/api/v2/account/market_order_history",
    "nonce": "1698489880"
}'
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "total": 2,                           // Total records in the queried range
        "orders": [
            {
                "id": 171366547790,           // Order id
                "amount": "0.00032",          // Original amount
                "price": "34293.92",          // Order price
                "type": "limit",              // Order type
                "side": "sell",               // Order side
                "ctime": 1698237533.497241,   // Order creation time
                "ftime": 1698237535.41196,    // Order fill time
                "market": "BTC_USDT",         // Market name
                "takerFee": "0.0018",         // Taker fee
                "makerFee": "0.0016",         // Market fee
                "dealFee": "0.01755848704",   // Deal fee
                "dealStock": "0.00032",       // Filled amount
                "dealMoney": "10.9740544"     // Filled total
            },
            {
                "id": 171366528218,
                "amount": "0.00034",
                "price": "34294.12",
                "type": "limit",
                "side": "buy",
                "ctime": 1698237524.638064,
                "ftime": 1698237524.638073,
                "market": "BTC_USDT",
                "takerFee": "0.0018",
                "makerFee": "0.0016",
                "dealFee": "0.02098800144",
                "dealStock": "0.00034",
                "dealMoney": "11.6600008"
            }
        ]
    }
}
```


##### Deals history by market

Query deals history. 
Only the transaction records in the past 3 month can be queried.


```
POST /api/v2/account/market_deal_history
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | BTC_USDT | Market name.
startTime | LONG | YES | 1698420600 |
endTime | LONG | YES | 1698507000 |
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.
limit | INT | NO | 100 | limit . Min value 1.  Default value 50 . Max value 100.


* The time between startTime and endTime can't be longer than 24 hours (86400 seconds).

**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/account/market_deal_history' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
    "market": "ETH_BTC",
    "startTime": 1698420600,
    "endTime": 1698507000,
    "limit": 100,
    "offset": 0,
    "request": "/api/v2/account/market_deal_history",
    "nonce": "1698507100"
}'
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "total": 2,                                 // Total records in the queried range
        "deals": [
            {
                "deal_id": 7450617292,              // Deal id
                "deal_time": 1698506956.66224,      // Deal execution time
                "deal_order_id": 171955225751,      // Deal order id
                "opposite_order_id": 171955110512,  // Opposite order id
                "side": "sell",                     // Deal side
                "price": "0.05231",                 // Deal price
                "amount": "0.002",                  // Deal amount
                "deal": "0.00010462",               // Total (price * amount)
                "deal_fee": "0.000000188316",       // Deal fee
                "role": "taker",                    // Role. Taker or maker
                "isSelfTrade": false                // is self trade
            },
            {
                "deal_id": 7450602006,
                "deal_time": 1698506366.865548,
                "deal_order_id": 171953919755,
                "opposite_order_id": 171953922008,
                "side": "sell",
                "price": "0.05232",
                "amount": "0.0036",
                "deal": "0.000188352",
                "deal_fee": "0.0000003013632",
                "role": "maker",
                "isSelfTrade": false
            }
        ]
    }
}
```

##### Deals by order ID

Get deal details for the filled order id.

```
POST /api/v2/account/order
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
orderId | INT | YES | 123456 | Order id;
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.
limit | INT | NO | 1 | limit . Min value 1.  Default value 50 . Max value 100.


**Request example:**

```
curl --location 'https://api.p2pb2b.com/api/v2/account/order' \
--header 'Content-Type: application/json' \
--header 'X-TXC-APIKEY: {{apiKey}}' \
--header 'X-TXC-PAYLOAD: {{payload}}' \
--header 'X-TXC-SIGNATURE: {{signature}}' \
--data '{
    "orderId": 171366547790,
    "limit": "50",
    "offset": "0",
    "request": "/api/v2/account/order",
    "nonce": "1698507307"
}'
```

**Response example:**
```json5
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "offset": 0,
        "limit": 50,
        "records": [
            {
                "id": 7429883128,             // Deal id
                "time": 1698237535.41196,     // Deal execution time
                "fee": "0.01755848704",       // Deal fee
                "price": "34293.92",          // Deal price
                "amount": "0.00032",          // Deal amount
                "dealOrderId": 171366551416,  // Deal order id
                "role": 1,                    // Deal role (1 - maker, 2 - taker)
                "deal": "10.9740544"          // Total (price * amount)
            }
        ]
    }
}
```

##### Order history

Query executed orders. 
The method will be removed soon.
Please, use instead [Orders history by market](#orders-history-by-market)

```
POST /api/v2/account/order_history
```

**Parameters:**

* Basic parameters should be added to the request body.


**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/order_history"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"request":"{{request}}","nonce":{{nonce}}}"
```


**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": [
        "ETH_BTC": [
              {
                "amount": "1",
                "price": "0.01",
                "type": "limit",
                "id": 9740,
                "side": "sell",
                "ctime": 1533568890.583023,
                "takerFee": "0.002",
                "ftime": 1533630652.62185,
                "market": "ETH_BTC",
                "makerFee": "0.002",
                "dealFee": "0.002",
                "dealStock": "1",
                "dealMoney": "0.01",
            }
        ],
        "ATB_USD": [
            {
                "amount": "0.3",
                "price": "0.06296168",
                "type": "market",
                "id": 11669,
                "source": "",
                "side": "buy",
                "ctime": 1533626329.696647,
                "takerFee": "0.002",
                "ftime": 1533626329.696659,
                "market": "ATB_USD",
                "makerFee": "0.002",
                "dealFee": "0.000037777008",
                "dealStock": "0.3",
                "dealMoney": "0.018888504",
            }
        ]
    ]
}
```



##### Executed history

List of executed deals for a market.
The method will be removed soon.
Please, use instead [Deals history by market](#deals-history-by-market)

```
POST /api/v2/account/executed_history
```

**Parameters:**

* Basic parameters should be added to the request body.


Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing enabled markets.
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.   
limit | INT | NO | 1 | limit . Min value 1.  Default value 50 . Max value 100.  


**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/executed_history"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"market":"ETH_BTC","limit":"1","offset":"0","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        [
            {
                "id": 726370
                "time": 1572345557.3056
                "side": "buy"
                "role": 2
                "amount": "0.5"
                "price": "1000"
                "deal": "500"
                "fee": "1"
            }, 
            {     
                "id": 726370
                "time": 1572345557.3056
                "side": "sell"
                "role": 1
                "amount": "0.5"
                "price": "1000"
                "deal": "500"
                "fee": "1"
            }
        ]
    }
}
```