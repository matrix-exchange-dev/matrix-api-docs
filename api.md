# Public Rest API for Matrix (2019-07-08)
# General API Information
* The base endpoint is: **https://api.matrix.com**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
# API Access
## Authentication
To protect API communication from unauthorized change, all non-public API calls are required to be signed.
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
| type |  K_1_HOUR |  Data type of K-line graph |
**Parameters：**

| Name  |  Required | Example  | Description  |
| ------------ | ------------ | ------------ | ------------ |
| end | No  |  1552176000000 |  endTime |
| start | No  |  1552100000000 |  startTime |
                                 |        |                                                              |
#### Response:

```
{
   “ lastUpdateId ”： 1027024，
   “bids”： [
    [
      “ 4.00000000 ”，      //PRICE
      “ 431.00000000 ”     //QTY
    ]
  ]
  “asks”： [
    [
      “ 4.00000200 ”，
       “ 12.00000000 ”
    ]
  ]
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

### Request Parameters

#### Response:

```
{
	"status": "success",
	"data": {
		"BCH_BTC": "[1562468623426,0.03583,0.03588,0.03486,0.0356,11.68]",
		"BCH_ETH": "[1562468623426,1.3981372,1.4023618,1.323264,1.3387682,162.74986]",
		"ETC_BTC": "[1562468623426,7.0E-4,7.0E-4,6.88E-4,6.89E-4,9.62]",
		"ETC_ETH": "[1562468623426,0.02715,0.02725,0.02563,0.02594,11706.949]",
		"ETH_BTC": "[1562468623426,0.02558,0.02673,0.0253,0.02673,11.095999]",
		"LTC_BTC": "[1562468623426,0.010339,0.01054,0.01024,0.010409,1.6769999]",
		"LTC_ETH": "[1562468623426,0.4106,0.4123,0.3844,0.3899,924.2485]",
		"ZRX_BTC": "[1562468623426,2.5578E-5,2.69E-5,2.5382E-5,2.6264E-5,241.0]",
		"ZRX_ETH": "[1562468623426,0.0010181,0.0010258,9.629E-4,9.958E-4,428.0]"
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
	"status": "success",
	"data": [{
		"amount": 0.088,
		"createdAt": 1.562554733033E12,
		"direction": false,
		"price": 0.02673,
		"symbol": "ETH_BTC"
	}, {
		"amount": 0.006,
		"createdAt": 1.562550770542E12,
		"direction": true,
		"price": 0.0266,
		"symbol": "ETH_BTC"
	}]
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
                "depositEnabled": true,
                "withdrawEnabled": true,
                "meta": {},
                "fex": 11944.577161968466316292
            },
            {
                "name": "AED",
                "legal": true,
                "tokenDecimals": 5,
                "depositEnabled": true,
                "withdrawEnabled": true,
                "meta": {},
                "fex": null
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
                "currency": "BTC",
                "available": "123599455.038563896960271500",
                "frozen": "733.760027621395800000",
                "locked": "0",
                "estimate": "1447648029967.10",
                "legal": false
            },
            {
                "currency": "CTC",
                "available": "123600000.463143040000000000",
                "frozen": "0.000000000000000000",
                "locked": "0",
                "estimate": "0.00",
                "legal": false
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
| type     | true     | string |  Order type                                                      |        | BUY_MARKET：市价买, SELL_MARKET：市价卖, BUY_LIMIT：限价买,SELL_LIMIT：限价卖 |
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

**响应：**

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





