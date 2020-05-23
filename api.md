# Public Rest API for Matrix (2020-05-22)
# General API Information
* The base endpoint is: **https://api.matrix.co**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
# API Access
## Authentication
To protect API communication from unauthorized change, all non-public API calls are required to be signed.

## Symbols
Supported symbols：ETH_BTC,BCH_BTC,LTC_BTC,LTC_ETH,BCH_ETH

# Public API Endpoints
## Market Data

### K-line chart data

**Path：** /v1/market/bars/{symbol}/{type}

**Method：** GET

### Request Parameters

**Path Parameters：**

| Name | Example  | Description  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |   |
| type |  K_1_HOUR |  Data type of K-line graph （K_1_SEC,K_1_MIN,K_1_HOUR,K_1_DAY,K_5_MIN,K_15_MIN,K_30_MIN,K_4_HOUR,K_8_HOUR,K_1_WEEK）|
**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| end | No  |  1552176000000 |  endTime |
| start | No  |  1552100000000 |  startTime |
                                 |        |                                                              |
#### Response:

```
{
    "status": "success",
    "data": {
        "bars": [
            "[1562155200000,0.03625,0.0363,0.036,0.036,0.66]",
            "[1562158800000,0.03599,0.03624,0.03587,0.03624,0.49]",
            "[1562162400000,0.03621,0.03621,0.03599,0.03602,0.2]",
            "[1562169600000,0.03611,0.03655,0.03607,0.03655,0.32]",
            "[1562173200000,0.03651,0.03712,0.03634,0.03712,0.67]",
            "[1562176800000,0.03716,0.03732,0.03661,0.03661,0.59]",
            "[1562180400000,0.03656,0.03688,0.03651,0.03651,0.15]",
            "[1562205600000,0.03664,0.03664,0.03664,0.03664,0.01]"
        ]
    }
}
```

### 24hr ticker price change statistics

**Path：** /v1/market/prices

**Method：** GET

**Interface Description：**

<p>Logic:<br>
&nbsp;&nbsp; price = bar[4]; // price<br>
&nbsp;&nbsp; change = (bar[4] - bar[1]) / bar[1]; // 24-hour variation<br>
&nbsp;&nbsp; amount = bar[5]; // Trading volume<br>
&nbsp;&nbsp; direction = change &gt;= 0; // Rise or fall</p>
&nbsp;&nbsp; ticks = bar[5] // The Number of Ticks</p>

### Request Parameters

#### Response:

```
{
    "status": "success",
    "data": {
        "BCH_BTC": "[1590054810803,0.030188,0.030188,0.030188,0.030188,0.0,0.0]",
        "BCH_ETH": "[1590054810803,1.63796008,1.63796008,1.63796008,1.63796008,0.0,0.0]",
        "ETH_BTC": "[1590054810803,0.02027,0.02027,0.02027,0.02027,0.0,0.0]",
        "LTC_BTC": "[1590054810803,0.003,0.003,0.003,0.003,0.0,0.0]",
        "LTC_ETH": "[1590054810803,0.3874,0.3874,0.3874,0.3874,0.0,0.0]"
    }
}
```

### Symbol price ticker

**Path：** /v1/market/snapshot/{symbol}

**Method：** GET

**Interface Description：**

### Request Parameters

**Path Parameters**

| Name | Example  | Description  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |   |

#### Response:

```
{
	"status": "success",
	"data": {
		"symbol": "ETH_BTC",
		"sequenceId": 1.0434486E7,
		"timestamp": 1.562328949641E12,
		"price": 0.02612,
		"buyOrders": [{
			"price": 0.0261,
			"amount": 0.264
		}, {
			"price": 0.02609,
			"amount": 0.176
		} {
			"price": 0.024,
			"amount": 1.147
		}],
		"sellOrders": [{
			"price": 0.02613,
			"amount": 0.696
		}, {
			"price": 0.02614,
			"amount": 0.256
		}, {
			"price": 0.02699,
			"amount": 0.2
		}]
	}
}
```

### Latest Transaction Price Interface

**Path：** /v1/market/ticks/{symbol}

**Method：** GET

**Description：**

### Request Parameters

**Path Parameters**

| Name | Example  | Description  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |   |
**Parameters**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| num | No  |   |   | Default 50 data

**Response：**

```
{
   "status":"success",
   "data":[
      {
         "amount":0.005,
         "createdAt":1.590041076902E12,
         "direction":false,
         "makerUserId":0.0,
         "price":0.02027,
         "symbol":"ETH_BTC",
         "takerUserId":0.0
      },
      {
         "amount":0.005,
         "createdAt":1.590040762094E12,
         "direction":false,
         "makerUserId":0.0,
         "price":0.02027,
         "symbol":"ETH_BTC",
         "takerUserId":0.0
      }
   ]
}
```

##  Currency and Symbol Information Query 

**Path：** /v1/market/trades

**Method：** GET

**Interface Description：**

### Request Parameters

#### Response:

```
{
    "status": "success",
    "data": {
        "currencies": [
            {
                "name": "BTC",
                "legal": false,
                "tokenDecimals": 8,
                "scale": 6,
                "depositEnabled": true,
                "withdrawEnabled": true,
                "meta": {},
                "fex": 9305.76
            },
            {
                "name": "BCH",
                "legal": false,
                "tokenDecimals": 8,
                "scale": 4,
                "depositEnabled": true,
                "withdrawEnabled": true,
                "meta": {},
                "fex": 236.01
            }
        ],
        "symbols": [
            {
                "name": "ETH_BTC",
                "baseName": "ETH",
                "baseScale": 4,
                "baseMinimum": 0.02,
                "quoteName": "BTC",
                "quoteScale": 8,
                "quoteMinimum": 0.2,
                "startTime": 0,
                "endTime": 4999999980000,
                "meta": {}
            },
            {
                "name": "CTC_BTC",
                "baseName": "CTC",
                "baseScale": 8,
                "baseMinimum": 0.03,
                "quoteName": "BTC",
                "quoteScale": 4,
                "quoteMinimum": 0.3,
                "startTime": 0,
                "endTime": 4999999980000,
                "meta": {}
            }
        ]
    }
}
```
## Account  
##### (All endpoints in this section require authentication)

### Get all Accounts of the Current User 

**API Key Permission：Read**

**Path：** /v1/user/accounts 

**Method：** GET

**Interface Description：**

### Request Parameters

#### Response:

```
{
    "status": "success",
    "data": {
        "accounts": [
            {
            "currency":"BTC",
            "available":"1.837988",
            "frozen":"0.149881",
            "locked":"0.000000",
            "usdEstimate":"18498.63",
            "btcEstimate":"1.987869",
            "legal":false,
            "scale":6
         },
         {
            "currency":"BCH",
            "available":"0.8100",
            "frozen":"0.0000",
            "locked":"0.0000",
            "usdEstimate":"191.17",
            "btcEstimate":"0.020542",
            "legal":false,
            "scale":4
         }
        ],
        "fiatTotalEstimate": {
            "currency": "USD",
            "value": "0.00"
        },
        "cryptoTotalEstimate": {
            "currency": "USD",
            "value": "1550853392470.83"
        }
    }
}
```

## Trading Interface
##### All endpoints in this section require authentication
### Place an order 
**API Key Permission：Trade**

**Path：** /v1/trade/orders 

**Method：** POST

**Interface Description：** Create new orders

### Request Parameters

**Parameters：**

| Name | Required | Type   | Description                                                         |  Default values  |  Range of values                                                      |
| -------- | -------- | ------ | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| amount   | true     | string | Limit order indicates the quantity of the order placed, and market price indicates how much to buy.（ At this point, the maximum accuracy is fixed to 8. ）， How much money do you sell on the market price list?  |        |                                                              |
| price    | false    | string |  Order price, market price list does not pass this parameter                                    |        |                                                              |
| source   | false    | string |  Source of order                                                      |        |                                                              |
| symbol   | true     | string |                                                        |        | BTC_USDT, ETH_BTC...                                         |
| type     | true     | string |  Order type                                                      |        | BUY_MARKET, SELL_MARKET, BUY_LIMIT, SELL_LIMIT |
|          |          |        |                                                              |        |                                                              |

#### Response:

```

    "status":"success",
    "data":{
        "createdAt":1562219810799,
        "updatedAt":1562219810799,
        "seqId":0,
        "previousSeqId":0,
        "refOrderId":0,
        "refSeqId":0,
        "userId":99028,
        "source":"",
        "symbol":"ETH_BTC",
        "sequenceIndex":13,
        "type":"BUY_LIMIT",
        "price":1,
        "amount":1,
        "filledAmount":0,
        "fee":0,
        "triggerOn":0,
        "makerFeeRate":0.0001,
        "takerFeeRate":0.0002,
        "chargeQuote":true,
        "features":0,
        "status":"SUBMITTED",
        "id":894,
        "feeCurrency":"BTC"
    }
}
```
###  Cancel orders  

**API Key Permission：Trade**

**Path：** /v1/trade/orders/{orderId}/cancel (HMAC SHA256)

**Method：** POST

**Interface Description：** Cancel an active order

### Request Parameters

**Path Parameters**

| Name | Example   | Description   |
| -------- | ------ | ------ |
| orderId  | 100006 | orderId |



#### Response:

```
{
    "status":"success",
    "data":{
        "createdAt":1562222968165,
        "updatedAt":1562222968165,
        "seqId":0,
        "previousSeqId":0,
        "refOrderId":879,
        "refSeqId":879,
        "userId":99028,
        "source":"CANCEL",
        "symbol":"BCH_BTC",
        "sequenceIndex":16,
        "type":"CANCEL_SELL",
        "price":247.52793709,
        "amount":0,
        "filledAmount":0,
        "fee":0,
        "triggerOn":0,
        "makerFeeRate":0,
        "takerFeeRate":0,
        "chargeQuote":true,
        "features":0,
        "status":"SUBMITTED",
        "id":895,
        "feeCurrency":"BTC"
    }
}
```


### 

### Query order 

**API Key Permission：Read**

**Path：** /v1/trade/orders/{orderId} 

**Method：** GET

**Interface Description：**


### Request Parameters

**Path Parameters**

| Name | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| orderId |  100003 |  orderId |

#### Response:

```
{
    "status": "success",
    "data": {
        "createdAt": 1560915105922,
        "updatedAt": 1560915118757,
        "seqId": 1,
        "previousSeqId": 0,
        "refOrderId": 0,
        "refSeqId": 0,
        "userId": 99000,
        "source": "",
        "symbol": "LTC_BTC",
        "sequenceIndex": 15,
        "type": "BUY_LIMIT",
        "price": 200.2342,
        "amount": 0.1,
        "filledAmount": 0.1,
        "fee": 0.018021078,
        "triggerOn": 0,
        "makerFeeRate": 0.0009,
        "takerFeeRate": 0.002,
        "chargeQuote": true,
        "features": 0,
        "status": "FULLY_FILLED",
        "id": 1,
        "feeCurrency": "BTC"
    }
}
```

### Get the Order Detail of an Order 

**API Key Permission：Read**

**Path：** /v1/trade/orders/{orderId}/matches 

**Method：** GET

**Interface Description：**


### Request Parameters

**Path Parameters**

| Name | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| orderId |  100009 |   Transacted Order Number  | 

**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| limit | No  |  10 |  limit |
| offsetId | No  |  0 |  offsetId |

#### Response:

```
{
    "status": "success",
    "data": {
        "hasMore": false,
        "matches": [
            {
                "id": 2,
                "createdAt": 1560915118759,
                "type": "MAKER",
                "price": 200.2342,
                "amount": 0.1,
                "feeCurrency": "BTC",
                "fee": 0.018021078
            }
        ],
        "nextOffsetId": 0
    }
}
```

###  Query all orders

**API Key Permission：Read**

**Path：** /v1/trade/orders 

**Method：** GET

**Interface Description：**


### Request Parameters

**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| limit | No  |  10 |  limit |
| offsetId | No  |  0 |  offsetId |
| symbol | No  |  ETH_USDT |  symbol |

#### Response:

```
{
    "status": "success",
    "data": {
        "hasMore": false,
        "nextOffsetId": 0,
        "orders": [
            {
                "createdAt": 1561640435237,
                "updatedAt": 1561640435237,
                "seqId": 0,
                "previousSeqId": 0,
                "refOrderId": 0,
                "refSeqId": 0,
                "userId": 99000,
                "source": "",
                "symbol": "LTC_ETH",
                "sequenceIndex": 19,
                "type": "BUY_LIMIT",
                "price": 1,
                "amount": 1,
                "filledAmount": 0,
                "fee": 0,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "takerFeeRate": 0.002,
                "chargeQuote": true,
                "features": 0,
                "status": "SUBMITTED",
                "id": 10841,
                "feeCurrency": "ETH"
            },
            {
                "createdAt": 1561639860285,
                "updatedAt": 1561639860285,
                "seqId": 0,
                "previousSeqId": 0,
                "refOrderId": 0,
                "refSeqId": 0,
                "userId": 99000,
                "source": "",
                "symbol": "LTC_ETH",
                "sequenceIndex": 19,
                "type": "BUY_LIMIT",
                "price": 1,
                "amount": 1,
                "filledAmount": 0,
                "fee": 0,
                "triggerOn": 0,
                "makerFeeRate": 0.001,
                "takerFeeRate": 0.002,
                "chargeQuote": true,
                "features": 0,
                "status": "SUBMITTED",
                "id": 10840,
                "feeCurrency": "ETH"
            }
        ]
    }
}
```

### Query orders 

**API Key Permission：Read**

**Path：** /v1/trade/orders-by-pagination  (HMAC SHA256) 

**Method：** GET

**Interface Description：**

### Request Parameters

**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| pageNumber | No  |  1 |  CurrentPage |
| pageSize | No  |  20 |   Number of bars displayed on a page  |
| symbol | No |  ETH_USDT |  symbol |
| start | No  |  2019-04-23 |   start time  |
| end | No  |  2019-05-23 |   Ending time  |
| orderType | No  |  SELL |   Trading Order Marking  |

### Response:

```
{
    "status": "success",
    "data": {
        "pagination": {
            "pageNumber": 1,
            "pageSize": 10,
            "total": 8
        },
        "items": [
            {
                "id": 8123,
                "chargeQuote": 1,
                "features": 0,
                "sequenceIndex": 0,
                "createdAt": 1560919968524,
                "previousSeqId": 8115,
                "refOrderId": 0,
                "refSeqId": 0,
                "seqId": 0,
                "updatedAt": 0,
                "userId": 0,
                "version": 0,
                "amount": 1.09278023,
                "fee": 0,
                "filledAmount": 0,
                "makerFeeRate": 0.001,
                "price": 0,
                "takerFeeRate": 0.002,
                "triggerOn": 0,
                "source": "",
                "status": "CANCELED",
                "symbol": "CTC/BTC",
                "type": "MARKET",
                "filledRate": 0,
                "averagePrice": 0,
                "side": "SELL"
            },
            {
                "id": 7314,
                "chargeQuote": 1,
                "features": 0,
                "sequenceIndex": 0,
                "createdAt": 1560919551942,
                "previousSeqId": 7312,
                "refOrderId": 0,
                "refSeqId": 0,
                "seqId": 0,
                "updatedAt": 0,
                "userId": 0,
                "version": 0,
                "amount": 0.6802,
                "fee": 0,
                "filledAmount": 0,
                "makerFeeRate": 0.0001,
                "price": 0,
                "takerFeeRate": 0.0002,
                "triggerOn": 0,
                "source": "",
                "status": "CANCELED",
                "symbol": "ETH/BTC",
                "type": "MARKET",
                "filledRate": 0,
                "averagePrice": 0,
                "side": "SELL"
            }
        ]
    }
}
```

### Get the Order Detail 

**API Key Permission：Read**

**Path：** /v1/trade/matchDetails-by-pagination  (HMAC SHA256) 

**Method：** GET

**Interface Description：**


### Request Parameters

**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| pageNumber | No  |  10 |   |
| pageSize | No  |  20 |   |
| symbol | No  |  ETH_USDT |   |
| start | No  |  2019-04-23 |   |
| end | No  |  2019-05-23 |   |
| orderType | No  |  SELL |   |

#### Response:

```
{
    "status": "success",
    "data": {
        "pagination": {
            "pageNumber": 1,
            "pageSize": 10,
            "total": 2
        },
        "items": [
            {
                "id": 9270,
                "createdAt": 1560918974270,
                "orderid": 6139,
                "userid": 99000,
                "amount": 0.4229,
                "fee": 0.1126422537589629,
                "price": 295.95190289,
                "feecurrency": "BTC",
                "type": "MARKET",
                "symbol": "LTC/BTC",
                "side": "BUY"
            },
            {
                "id": 5670,
                "createdAt": 1560917758344,
                "orderid": 3773,
                "userid": 99000,
                "amount": 0.1441,
                "fee": 0.0033884523708706,
                "price": 235.14589666,
                "feecurrency": "BTC",
                "type": "LIMIT",
                "symbol": "ETH/BTC",
                "side": "BUY"
            }
        ]
    }
}
```





