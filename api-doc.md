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
    - [Account](#account)
       - [Balances](#balances)
       - [Currency balance](#currency-balance)
    - [Orders](#orders)
       - [Order history](#order-history)
       - [Order deals](#order-deals)
       - [Executed history](#executed-history)
       - [All executed history](#all-executed-history)
       - [Order list](#order-list)
       - [Create order](#create-order)
       - [Cancel order](#cancel-order)
     <!--  - [Cancel all orders](#cancel-all-orders) -->

# Websocket API

<https://p2pb2bwsspublic.docs.apiary.io/>

# Rest API for P2PB2B


## API Information

* Base URL for requests is <https://api.p2pb2b.com>

The following rules may apply to DDoS attacks on API endpoints:

* If an IP address exceeds 5 requests per second and 100 requests per minute to a specific REST API endpoint e.g., /api/v2/account/balance, the requesting IP address will be blocked for 1 minute. 

Please note the exact logic and handling for such DDoS defenses may change over time to further improve the reliability.

## API Endpoints

Possible errors when executing queries: 

* [Possible error](./errors.md)

### Public

Public folder requests available without authentication.

**General requirements**

* All requests to a public API are executed by the GET method. With or without parameters passed to the URL.
* Parameters may be required (optional).
* More information can be found in the description of a specific endpoint.

**Info**

* **offset**  Default value = 0. Value change step = 50. The passed parameter will be selected according to these requirements. The nearest larger value will be set, which is a multiple of the change step.

* **limit**  Value change step = 50. The passed parameter will be casted to these requirements. The nearest larger value will be set, which is a multiple of the change step, but not bigger than the allowed maximum.

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

The request returns a json with '[market](./resources.md#markets)' items listed. More details could be found in [resources](./resources.md).


**Response example:**
```javascript
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": [
        {
            "name":"ETH_BTC",
            "stock":"ETH",
            "money":"BTC",
            "precision": {
                "money":"6",
                "stock":"3",
                "fee":"4"
            },
            "limits": {
                "min_amount":"0.001",
                "max_amount":"100000",
                "step_size":"0.001",
                "min_price":"0.000001",
                "max_price":"100000",
                "tick_size":"0.000001",
                "min_total":"0.0001"
            }
        },
        {
            "name":"ETH_BTC",
            "stock":"ETH",
            "money":"BTC",
            "precision": {
                "money":"6",
                "stock":"3",
                "fee":"4"
            },
            "limits": {
                "min_amount":"0.001",
                "max_amount":"100000",
                "step_size":"0.001",
                "min_price":"0.000001",
                "max_price":"100000",
                "tick_size":"0.000001",
                "min_total":"0.0001"
            }
        },
    ]
}
```
#### Market

Get info by market.

```
GET /api/v2/public/market
```

**Parameters:**

Name | Type | Mandatory | Example | Description
------------ | ------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing included markets

**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/market?market=ETH_BTC"

```

The request returns a json with '[market](./resources.md#markets)' item. More details could be found in  [resources](./resources.md).


**Response example:**
```javascript
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "name":"ETH_BTC",
        "stock":"ETH",
        "money":"BTC",
        "precision": {
            "money":"6",
            "stock":"3",
            "fee":"4"
        },
        "limits": {
            "min_amount":"0.001",
            "max_amount":"100000",
            "step_size":"0.001",
            "min_price":"0.000001",
            "max_price":"100000",
            "tick_size":"0.000001",
            "min_total":"0.0001"
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

**Resource**

The request returns a json with '[ticker](./resources.md#tickers)' items list. More details could be found in [resources](./resources.md).


**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": [
        "ETH_BTC": {
            "at": 1546456422,
                "ticker": {
                "bid": "0.03871781",
                "ask": "0.03921116",
                "low": "0.03654814",
                "high": "0.03952763",
                "last": "0.039",
                "vol": "59161.64311441",
                "deal": "26.185802798",
                "change": "-3.25" 
            }
        },
        "BTC_USD": {
            "at": 1546456422,
            "ticker": {
                "bid": "4106",
                "ask": "4150",
                "low": "4073.00010001",
                "high": "4142.8685",
                "last": "4140.989",
                "vol": "2613.29685416",
                "deal": "44569811.52006821",
                "change": "1.09" 
            }
        },
    ]
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
market | STRING | YES | ETH_BTC | Market name from the list of existing included markets


**Сaching:**

Results are cached for ~30s


**Request example:**

```
curl --location --request GET "http://api.p2pb2b.com/api/v2/public/ticker?market=ETH_BTC"

```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        "bid": "0.03712579",
        "ask": "0.039799",
        "open": "0.03931012",
        "high": "0.03992342",
        "low": "0.03889204",
        "last": "0.03914",
        "volume": "58459.37131464",
        "deal": "2267.6624026872515709",
        "change": "-0"
    }
}
```

#### Order book

Get all unexecuted orders by market.

```
GET /api/v2/public/book
```

**Parameters:**

Name | Type | Mandatory | Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing included markets
side | STRING | YES | buy | Type of order. Valid values: 'sell, 'buy' 
offset | INT | NO | 0 | offset . Min value 0. Default 0. Max value 10000.   
limit | INT | NO | 1 | limit . Min value 1.  Default value 50 . Max value 100.  




**Сaching:**

Results are cached for ~1s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/book?market=ETH_BTC&side=sell&offset=0&limit=100"

```

The request returns a json with '[order book](./resources.md#order-book)' items list. More details could be found in [resources](./resources.md).

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
    "offset": 0,
    "limit": 100,
    "total": 422,
    "orders": [
        {
            "id": 3951818,
            "left": "0.2429769",
            "market": "ETH_BTC",
            "amount": "0.2429769",
            "type": "limit",
            "price": "0.039799",
            "timestamp": 1546505184.394757,
            "side": "sell",
            "takerFee": "0.002",
            "makerFee": "0.002",
            "dealStock": "0",
            "dealMoney": "0"
        },
        {
            "id": 3951396,
            "left": "0.12592592",
            "market": "ETH_BTC",
            "amount": "0.12592592",
            "type": "limit",
            "price": "0.04149",
            "timestamp": 1546504746.051004,
            "side": "sell",
            "takerFee": "0",
            "makerFee": "0",
            "dealStock": "0",
            "dealMoney": "0"
        },
    ]
}
```

#### History

Each order history starts with order ID.

```
GET /api/v2/public/history
```

**Parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing included markets
lastId | INT | YES | 123456 | Executed order id
limit | INT | NO | 1 |  Limit . Min value 1.  Default value 50 . Max value 100.  

**Сaching:**

Results are cached for ~5s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/history?market=ETH_BTC&lastId=1&limit=100"

```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": [
        {
            "id": 1565088,
            "type": "sell",
            "time": 1546505899.001003,
            "amount": "84.51887027",
            "price": "0.03913999"
        },
        {
            "id": 1565048,
            "type": "buy",
            "time": 1546505770.311739,
            "amount": "52.65708334",
            "price": "0.03914"
        },
    ]
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
market | STRING | YES | ETH_BTC | Market name from the list of existing included markets
limit | INT | NO | 1 |  Limit . Min value 1.  Default value 50 . Max value 100.  
interval | ENUM  | NO | 0 |   Valid values: 0, 0.00000001, 0.0000001, "0.000001, 0.00001, 0.0001, 0.001, 0.01, 0.1, 1. Default 0;  
 

**Сaching:**

Results are cached for ~1s


**Request example:**

```
curl --location --request GET "https://api.p2pb2b.com/api/v2/public/depth/result?market=ETH_BTC&limit=100"

```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        "asks": [
            [
             "0.0393208",
             "0.07943891"
            ],
            [
             "0.0393212",
             "0.27257273"
            ],
        ],
        "bids": [
            [
             "0.03883456",
             "0.0367167"
            ],
            [
             "0.03882735",
             "1.77587772"
            ],
        ]
    }
}
```

#### Kline
```
GET /api/v2/public/market/kline
```
Kline/candlestick bars for a market.
Kline identification takes place at the opening time.

**Parameters:**

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing enabled markets.
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
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": [
        {
            1573472164,  // Open time
            "0.021421",  // Open
            "0.021407",  // Close
            "0.021427",  // Highest 
            "0.021402",  // Lowest
            "1000",      // Volume
            "123456.78", // Amount
            "ETH_BTC"    // Market name
        },
    ]
}
```


### Protected

For requests in Protected folder you should provide apiKey and apiSecret. Use this step-by-step list:

* Activate 2FA in your P2PB2B account in Account>Security <https://p2pb2b.com/account/security>
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

**nonce** - integer, preferably timestamp in milliseconds. Repeated nonces within the last 10 seconds are not allowed.

* Each endpoint may also have additional parameters. More information can be found in the description of a specific endpoint.

* The number of user requests to the endpoints of the protected API is limited. Not more than 10 requests per second.

#### Account

##### Balances

List of user balances for all currencies.

```
POST /api/v2/account/balances
```

**Parameters:**

* Basic parameters should be added to the request body.
* Basic parameters only.


The request returns a json with '[balance](./resources.md#markets)' items list. More details could be found in [resources](./resources.md).

**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/balances"
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
    "errorCode": "",
    "message": "",
    "result": {
        "ETH": {
            "available": "0",
            "freeze": "0"
        }
        "ETH": {
            "available": "0",
            "freeze": "0"
        }
    }
}
```
##### Currency balance

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
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/balance"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"currency":"ETH","request":"{{request}}","nonce":{{nonce}}}"

```

The request returns a json with '[balance](./resources.md#balances)' item. More details could be found in [resources](./resources.md).

**Response example:**
```javascript
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "ETH": {
            "available": "0",
            "freeze": "0"
        }
    }
}
```

#### Orders

##### Order history

Query executed orders.

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

The request returns a json with '[order history](./resources.md#order-history)' items list. More details could be found in [resources](./resources.md).

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
##### Order deals

Query deal details by executed order ID.

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
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/order"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"orderId":"123456","limit":"50","offset":"0","request":"{{request}}","nonce":{{nonce}}}"
```

The request returns a json with '[order deals](./resources.md#order-deals)' items list. More details could be found in [resources](./resources.md).

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        "offset": 0,
        "limit": 50,
        "records": [
            {
                "time": 1533310924.935978,
                "fee": "0",
                "price": "80.22761599",
                "amount": "2.12687945",
                "id": 548,
                "dealOrderId": 1237,
                "role": 1,
                "deal": "170.6344677716224055"
            }
        ]
    }
}
```

##### Executed history

List of executed user orders for a market.

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

##### All executed history

List of executed user orders for all markets.

```
POST /api/v2/account/executed_history/all
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
limit | INT | NO | 50 | Offset . Min value 0. Default 0. Max value 10000.   
offset | INT | NO | 0 | Limit . Min value 1.  Default value 50 . Max value 100.   

**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/account/executed_history/all"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"limit":"10","offset":"0","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        [
            "ETH_BTC": [
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
            ],
            "DOGE_BTC": [
                {
                    "id": 5177235
                    "time": 1569588463.2522
                    "side": "buy"
                    "role": 1
                    "amount": "2000"
                    "price": "0.00135"
                    "deal": "2.7"
                    "fee": "0.0054"
                }
            ]    
        ]
    }
}
```

##### Order list

Query unexecuted orders.

```
POST /api/v2/orders
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
curl --location --request POST "https://api.p2pb2b.com/api/v2/orders"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"market":"ETH_BTC","limit":"50","offset":"0","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
    "limit": 100,
    "offset": 0,
    "total": 1,
    "result": [
        {
            "id": 3900714,
            "left": "1",
            "market": "ETH_BTC",
            "amount": "1",
            "type": "limit",
            "price": "0.008",
            "timestamp": 1546459568.376407,
            "side": "buy",
            "dealFee": "0",
            "takerFee": "0.001",
            "makerFee": "0.001",
            "dealStock": "0",
            "dealMoney": "0"
        },
        {
            "id": 3900715,
            "left": "1",
            "market": "ETH_BTC",
            "amount": "1",
            "type": "limit",
            "price": "0.008",
            "timestamp": 1546459568.376429,
            "side": "buy",
            "dealFee": "0",
            "takerFee": "0.001",
            "makerFee": "0.001",
            "dealStock": "0",
            "dealMoney": "0"
        }
    ]
}
```

##### Create order

Сreating an order for a trade.


```
POST /api/v2/order/new
```

**Parameters:**

* Basic parameters should be added to the request body.


Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing enabled markets.
side | STRING | YES | buy | Type of order. Valid values: 'sell, 'buy' 
amount | STRING | TES | 10 | Amount
price | STRING | YES | 0.004846 | Price

* Orders that do not pass current limits will not be created. Limits are indicated in the markets.

* **amount**,**price** - must contain a numeric string.

**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/order/new"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"market":"ETH_BTC","side":"buy","amount":"0.001","price":"0.001","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
  "success": true,
   "errorCode":"",
  "message": "",
  "result": {
    "orderId": 25749,
    "market": "ETH_BTC",
    "price": "0.1",
    "side": "sell",
    "type": "limit",
    "timestamp": 1537535284.828868,
    "dealMoney": "0",
    "dealStock": "0",
    "amount": "0.1",
    "takerFee": "0.002",
    "makerFee": "0.002",
    "left": "0.1",
    "dealFee": "0"
  }
}
```

##### Cancel order

Cancel an active order for a market by order ID.

```
POST /api/v2/order/cancel
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing enabled markets.
orderId | INT | YES | 25749 | Order id for cancel


**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com/api/v2/order/cancel"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"market":"ETH_BTC", "orderId":"123456","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
    "success": true,
    "errorCode":"",
    "message": "",
    "result": {
        "orderId": 25749,
        "market": "ETH_BTC",
        "price": "0.1",
        "side": "sell",
        "type": "limit",
        "timestamp": 1537535284.828868,
        "dealMoney": "0",
        "dealStock": "0",
        "amount": "0.1",
        "takerFee": "0.002",
        "makerFee": "0.002",
        "left": "0.1",
        "dealFee": "0"
    }
}
```
<!--


##### Cancel all orders

Cancel all active orders for the 'market'.

```
POST /api/v2/order/cancel/all
```

**Parameters:**

* Basic parameters should be added to the request body.

Name|Type|Mandatory| Example | Description
------------ |------------ | ------------ | ------------ | ------------
market | STRING | YES | ETH_BTC | Market name from the list of existing enabled markets.


**Request example:**

```
curl --location --request POST "https://api.p2pb2b.com//api/v2/order/cancel/all"
  --header "Content-Type: application/json" \
  --header "X-TXC-APIKEY: {{apiKey}}" \
  --header "X-TXC-PAYLOAD: {{payload}}" \
  --header "X-TXC-SIGNATURE: {{signature}}" \
  --data "{"market":"ETH_BTC","request":"{{request}}","nonce":{{nonce}}}"
```

**Response example:**
```javascript
{
    "success": true,
    "errorCode": "",
    "message": "",
    "result": {
        "order_ids": [
             20065506,
             20065505
        ]
    }
}
```
-->
