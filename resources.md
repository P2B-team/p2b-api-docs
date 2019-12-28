# Resources

## Balances

```javascript
    "{{currency}}": {
        "available": "Currency amount available for trading.",
        "freeze": "Amount frozen during operations."
    }
```

* **{{currency}}** - currency token on which the balance is displayed.

## Markets

```javascript
{
    "name":"Market name.", 
    "stock":"Stock ticker.",
    "money":"Money ticker.",
    "precision": 
        {
            "money":"Precision for money currency.",
            "stock":"Precision for stock currency.",
            "fee":"Precision for fee"
        },
    "limits": 
        {
            "min_amount":"Minimum value of the amount.",
            "max_amount":"Maximum value of the amount.",
            "step_size":"Allowable step for changing the amount.",
            "min_price":"Minimum value of the price.",
            "max_price":"Maximum value of the price.",
            "tick_size":"Allowable step for changing the price.",
            "min_total":"Minimum value of the total."
        }
}
```
*  Limits are used as restrictions when creating new orders.

*  If the limit is 0, then this limit does not apply to this market.

* **step_size** - defines the intervals by which it is possible to change (increase / decrease) the amount in the range from **min_amount** to **max_amount**.

* **tick_size** - defines the intervals by which it is possible to change (increase / decrease) the price in the range from **min_price** to **max_price**.

* **min_total** - minimum allowed order total. 


## Tickers

```javascript
"{{market_name}}": {       
    "at": "Timestamp for transmitted data.",
    "ticker": {
        "bid":  "The bid price or the highest purchase price", 
        "ask":  "The offer price or the lowest selling price.",
        "low":  "The low price.",
        "high": "The high price.",
        "last": "The last price.",
        "vol":  "Volume of Trade.",
        "change": "Changing price (in percents)." 
    }
}
```
* **{{market_name}}** - for each data item, the key is determined in accordance with the market to which the data relates.

* ticker item endpoint  have additional fields : **open** - opening price, **deal** - deal;

## Orders

### Order history

```javascript
"{{market_name}}": [
    {
        "amount": "Order amount.",
        "price": "Order price.",
        "type": "Order type.",
        "id": "Order id.",
        "side": "Order transaction direction.",
        "ctime": "Time of creation.",
        "takerFee": "Taker fee.",
        "ftime": "Time of finish.",
        "market": "Market name.",
        "makerFee": "Maker fee.",
        "dealFee": "Deal fee.",
        "dealStock": "Volume for stock currency.",
        "dealMoney": "Volume for money currency.", 
    }
]
```

* **{{market_name}}** - for each data item, the key is determined in accordance with the market to which the data relates.

### Order book 


```javascript
{
    "id": "Order id.",
    "left": "Residual order amount",
    "market": "Market name.",
    "amount": "Order amount.",
    "type": "Order type",
    "price": "Order price.",
    "timestamp": "Time",
    "side": "Order side.",
    "dealFee": "Order deal fee",
    "takerFee": "Taker fee",
    "makerFee": "Maker fee",
    "dealStock": "Volume for stock currency.",
    "dealMoney": "Volume for money currency."
}
```

### Order deals 

```javascript
{
    "time": "Time",
    "fee": "Order Fee",
    "price": "Order price.",
    "amount": "Order amount.",
    "id": "Order id.",
    "dealOrderId": "Deal order id",
    "role": "Role. 1 - taker, 2 - maker",
    "deal": "Deal"
}
```
