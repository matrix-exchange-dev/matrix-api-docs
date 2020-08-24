# Access preparation
If you want to use API, please login to the webpage first, finish the application of API key and permission configuration and then develop and trade according to the details of this document. 

Each user can create 5 API Key and each API Key corresponds to setting two kinds of permission: Read and Trade.  

<strong>All the data about frequency in the document is likely to be adjusted.</strong>

## Permission description as follows:

Read permission: Read permission is used for data query interface, for example: order query, transaction query, etc.  

Trade permission: Trade permission is used for interface of placing orders and cancelling orders.  

After successful creation, please remember the following information:  


Access Key: The key used in API request  

Secret Key: The key used for signature authentication and encryption(Only visible at the point when you successfully create 2 strings of field)

 <strong>It is requested to bind IP address when creating API Key. After boundation, you can use trading interface. You cannot access the interface with an unbounded IP address.</strong>

## Access type

Matrix provides two kinds of interface for uses, so you can choose an appropriate way to query the market and trade according to your usage scenario and your preference.

#### REST API
REST, the abbreviation for Representational State Transfer, is a currently popular communication mechanism based on HTTP. Each URL represents a kind of resource.

For one-time operation like trade, developers are recommended to use REST API.

#### WebSocket API
WebSocket is a new protocol of HTML5. It realizes full-duplex communication between client and server. It can establish the connection between client and server through one simple handshake. Server can actively push information to client according to business rules. 

Developers are recommended to use WebSocket API to achieve information like market data, trade depth, etc.

#### Interface authentication

1. Rest API and Websocket API both contain public interface and private interface.

2. Public interface can be used to obtain basic information and market data. Public interface can be used without authentication.

3. Private interface can be used to manage transaction and account. Every private request must use your API Key for signature authentication.

#### Access URLs

REST API

https://zeus.matrix.co/

Websocket Feed(Market data)

https://zeus-wss.matrix.co/

#### Signature authentication

API request is very likely to be tampered during the transmission through internet. In order to ensure the request not tampered, all the private interface except public interface (basic information and market data) must use your API Key for signature authentication to verify whether parameter or parameter value is changed during the transmission. 
Each API Key need appropriate permission to access the corresponding interface. Each newly created API Key needs to assign permission. Before using the interface, please check the permission type for each interface and confirm that your API Key has the corresponding permission.


A valid request is consisted of the following parts:

API path:Access the server address zeus.matrix.co，for example zeus.matrix.co/v1/order/orders.
API access Id (API-KEY) : Access Key in your API Key applied.
API-SIGNATURE-METHOD:Hash-based signature authentication calculation, HmacSHA256 is used here
API-SIGNATURE-VERSION:The version of signature protocol, version 1 is used here.
API-TIMESTAMP:Timestamp of your request time(<strong>the difference between request time and current standard cannot be greater than 1min or this will be regarded as invalid</strong>).   
For Get request, parameters that come with method need signature calculation.  
For POST request, parameters need to be put in body.  
Signature: the value calculated by signature. It is used to ensure that signature is valid and is not tampered.

###### Signature step
Standardize the request that need to calculate signature. Because when using HMAC to calculate signature, calculating with different content, the result obtained will be completely different. Therefore, before calculating signature, please standardize the request first. The following is an example of query request for details of a certain order

Query the complete request URL, when searching for details of a certain order

https://api.matrix.co/v1/order/orders?orderId=1234567890

1. Request method(GET or POST, WebSocket using GET)，followed by a newline character “n”

    For example: GET\n

2. Add the lowercase access domain name, followed by a newline character “n”

    For example: zeus.matrix.co\n

3. The path of access method, followed by a newline character “n”

    Such as query an order:
    /v1/order/orders\n

4. Encrypt Header information

    API-KEY=AAAADAVYcB2SNK5wFIAMHA2E \n
    
    API-SIGNATURE-METHOD=HmacSHA256 \n
    
    API-SIGNATURE-VERSION=1 \n
    
    API-TIMESTAMP=1593516127982 \n

5. According to the order above, connect the parameters

    POST\nzeus.matrix.co\n/v1/order/orders\nAAAADAVYcB2SNK5wFIAMHA2E\nHmacSHA256\n1\n1593516127982

6. If it is a GET request

    Add corresponding request parameter at the end of the order above,
    For example: /v1/order/orders?currency=BTC, then the corresponding complete signature is
    GET\nzeus.matrix.co\n/v1/order/orders\nAAAADAVYcB2SNK5wFIAMHA2E\nHmacSHA256\n1\n1593516127982\ncurrency=BTC
    
    For example: /v1/order/trade/ticks/BTC_USD  ,then the corresponding complete signature is
    GET\nzeus.matrix.co\n/v1/order/trade/ticks/BTC_USD\nAAAADAVYcB2SNK5wFIAMHA2E\nHmacSHA256\n1\n1593516127982

7. If it is a POST request

    Add corresponding JSON request body at the end of the order above

    For example: /v1/order/orders/place   
    body is
    ```
        {
            "price":"1.0000",
            "amount":"1",
            "type":"BUY_LIMIT",
            "symbol":"ETH_USD",
            "triggerOn":"1.0000"
        }
    ```
    then the corresponding complete signature is
    POST\nzeus.matrix.co\n/v1/order/orders/place\nAAAADAVYcB2SNK5wFIAMHA2E\nHmacSHA256\n1\n1593516127982\namount=1&price=1.0000&symbol=ETH_USD&triggerOn=1.0000&type=BUY_LIMIT
    
    Note:  
    In the body part of the POST request, the code will be parsed according to the positive order of acsii code  
    
    In the parameter part of the GET request, the code will be parsed according to the positive order of acsii code

8. Use the "request string" generated in the previous step and your Secret Key to generate an electronic signature 

    Regard the request string generated in the previous step and API private key as two parameters and use HmacSHA256 Hash functions to get Hash value
    Encode the Hash value with base-64. The value obtained is used as the eletronic signature of this interface--
    4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=

9. Add the electronic signature into the request

    For Rest interface:
    
    Add all the parameters that must be verified into the Header parameters of interface
    After URL encoding, add the electronic signature into the Header parameters, with parameter name "API-SIGNATURE".
    Finally, the API request sent to the server should be
    https://zeus.matrix.co/v1/order/orders?orderId=1234567890
    
    headers:     
    ```
        API-KEY:AAAADAVYcB2SNK5wFIAMHA2E
        API-SIGNATURE-METHOD: HmacSHA256
        API-SIGNATURE-VERSION: 1
        API-TIMESTAMP: 1593516127982
        API-SIGNATURE: 4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=
    ```


## API description

##### 1. Information of account balance
API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 5 times/s

Query all the account information related to the account of current user
HTTP request:
· GET /v1/account/accounts/balance

##### Request parameter:
None

##### Response Content:
| Parameter name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
|currency | true  |  string | currency type |        |
|available| true  |  string| avaliable balance| |
|frozen|true|string|amount blocked| |
|scale|true|int|precision| |  
```
Example: 
URL: //zeus.matrix.co/v1/account/accounts/balance
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": [{
    "currency": "BTC",
    "available": "123600491.35261088",
    "frozen": "183.3082",
    "scale": 8
  }, {
    "currency": "ETC",
    "available": "123600000",
    "frozen": "0",
    "scale": 4
  }, {
    "currency": "BCH",
    "available": "123597403.1418",
    "frozen": "2320.2",
    "scale": 6
  }, {
    "currency": "USD",
    "available": "123599955.63824",
    "frozen": "96.192",
    "scale": 6
  }, {
    "currency": "ETH",
    "available": "123597479.07025",
    "frozen": "2418.83266",
    "scale": 6
  }, {
    "currency": "LTC",
    "available": "123599906.228",
    "frozen": "75.236",
    "scale": 4
  }]
}
```

##### 2. Information of account balance (single currency)
API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 5 times/s

Query the balance information of the user according to the current currency. Only support single currency query.

HTTP request:
· GET /v1/account/accounts/balance/{currency}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description |
|---| --- | --- | --- | --- |
|currency| string | true | NA | Currency queried, like BTC|

##### Response Content:
| Parameter name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
|currency | true  |  string | currency type |        |
|available| true  |  string| available|  |
|frozen|true|string|amount blocked| |
|scale|true|int|precision| |  
```
Example: 
URL: //zeus.matrix.co/v1/account/accounts/balance/USD
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": [{
    "currency": "USD",
    "available": "123599955.63824",
    "frozen": "96.192",
    "scale": 6
  }]
}

```

##### 3. Place order
API Key permission: Trade  
Rate limiting value(NEW): 5 times/s

HTTP request:
· POST /v1/order/orders/place

##### Request parameter:
| Parameter | Data type | Required | Default | Description|
|---| --- | --- | --- | --- | 
|symbol| string | true | NA | currency pair traded, like BTC_USD (Need capital to exactly match)|
|type|string|true|NA|transaction type, like: BUY_LIMIT (Need capital to exactly match)|
|amount|string|false|0| order size|
|price|string|true|NA| BUY_LIMIT or SELL_LIMIT represents unit price, BUY_MARKET or SELL_MARKET represents total price，|
|triggerOn| string | false | 0 | trigger price|
|clientOrderId| string | false | NA | User-defined order number | |

##### Response Content:
| Parameter name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
|orderId | true  |  string | Order ID of orders successfully placed in Matrix system  |        |
|clientOrderId| false  |  string | User-defined order number|  | |
```
Example:
URL://zeus.matrix.co/v1/order/orders/place
Above is the request URL, and below is the request parameter.
{
    "price": "3.5",
    "amount": "1.8",
    "type": "BUY_LIMIT",
    "symbol": "LTC_BTC",
    "total": "3","clientOrderId":"1596786438902"
}
Below is the response result.
{
  "status": "success",
  "data": {
    "clientOrderId": "1596786438902",
    "orderId": "135163"
  }
}
```

##### Description:
1. clientOrderId

    Since Matrix will not promot rerangement tips, clientOrderId user need to guanrantee that this ID is not repetitive. If there is repetition, when you cancel or query orders, you can only cancel or query the latest piece of data.

2. type, price, amount and triggerOn field
    *  limit price order: When the user order type is limit price order:
    <strong>type</strong> is BUY_LIMIT or SELL_LIMIT,
    <strong>amount</strong> is a required field at this time,
    <strong>price</strong> is unit price at this time
    
    For example:
    ```
    {"price":"1.0000","amount":"0","type":"BUY_LIMIT","symbol":"ETH_BTC"}
    ```
    * market order: When the user order type is market price order:
    <strong>type</strong> is BUY_MARKET or SELL_MARKET,
    <strong>amount</strong> is not required at this time,
    <strong>price</strong> is total price at this time. For example, if a user wants to buy 1 BTC, when currency pair is BTC_USD and market price is 9000, he can directly enter 9000.
    
    For example:
    ```
    {"price":"1.0000","type":"BUY_MARKET","symbol":"ETH_USD"}
    ```

    * stop-limit: When the user order type is stop-limit order:
    <strong>type</strong> is BUY_LIMIT or SELL_LIMIT,
    <strong>amount</strong> is required field at this time,
    <strong>price</strong> is unit price at this time
    <strong>triggerOn</strong> is required field at this time. If this is empty or trgigerOn is 0, this order is regarded as limit price order by default.
    
    For example:
    ```
    {
        "price":"1.0000",
        "amount":"1",
        "type":"BUY_LIMIT",
        "symbol":"ETH_USD",
        "triggerOn":"1.0000"
    }
    ```

##### 4. Place a batch of orders
API Key permission: Trade  
Rate limiting value(NEW): 5 times/s

HTTP request:
· POST /v1/order/orders/batch-place

##### Request parameter:
| Parameter | Data type | Required | Default | Description|
|---| --- | --- | --- | --- |
|[{symbol| string | true | NA | Currency pair traded, like: BTC_USD (Need capital to exactly match)|
|type|string|true|NA|Transaction type, like: BUY_LIMIT (Need capital to exactly match)|
|amount|string|false|0| order size; When place market sell orders, it is possible that there is no amount|
|price|string|true|NA| BUY_LIMIT or SELL_LIMIT represents unit price，BUY_MARKET or SELL_MARKET represents total price; When place market sell orders, it is possible that there is no amount|
|triggerOn| string | false | 0 | trigger price|
|clientOrderId}]| string | false | NA | User-defined order number |

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
|[{orderId | true  |  string | Order ID of orders successfully placed in Matrix system |        |
|clientOrderId}]| false  |  string | ser-defined order number|  | | 
```
The maximum number of orders placed is 10.
Example:
URL: //zeus.matrix.co/v1/order/orders/batch-place
Above is the request URL, and below is the request parameter.
[{ "price": "4.3",
    "amount": "2.10",
    "type": "SELL_LIMIT",
    "symbol": "BTC_USD",
    "total": "3","clientOrderId":"1596786029042"},
    { "price": "4.1",
    "amount": "1.5",
    "type": "BUY_LIMIT",
    "symbol": "BCH_BTC",
    "total": "3","clientOrderId":"15967860290421"},
    { "price": "4.4",
    "amount": "1.3",
    "type": "SELL_LIMIT",
    "symbol": "LTC_ETH",
    "total": "3","clientOrderId":"15967860290422"}]
Below is the response result.
{
  "status": "success",
  "data": {
    "success": [{
      "clientOrderId": "1596786029042",
      "orderId": "135139"
    }, {
      "clientOrderId": "15967860290421",
      "orderId": "135140"
    }, {
      "clientOrderId": "15967860290422",
      "orderId": "135141"
    }],
    "error": []
  }
}
```

##### 5. Submit cancel for an order
API Key permission: Trade  
Rate limiting valuew(NEW): 5 times/s

HTTP request:
· POST /v1/order/orders/cancel

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|orderId| string | true | NA | Order ID that user would like to cancel|

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| orderId | true | string | Order ID that user successfully cancel|  |
```
Example: 
URL: //zeus.matrix.co/v1/order/orders/cancel
Above is the request URL, and below is the request parameter.
{"clientOrderId":"1596786999151","orderId":"135201"}
Below is the response result.
{
  "status": "success",
  "data": {
    "clientOrderId": null,
    "id": "135201",
    "orderId": "135201.0"
  }
}
```

##### 6. Submit cancel for a batch of orders
API Key permission: Trade  
Rate limiting value(NEW): 5 times/s

HTTP request:
· POST /v1/order/orders/batch-cancel

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|orderIds| string | false | NA | Either order-ids or clientOrderIds can be filled in one batch request, separated by ',' and no more than 50 items at a time|
|clientOrderIds| string | false | NA | Either User-defined ID and orderIds can be filled in one batch request, separated by ',' and no more than 50 items at a time|

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| orderId | false | string | Order ID of cancel request failled | |
| error-code | false | string | The reason for a failled cancel request. When orders in batches are placed, if one of the actions fails, and then jump this failed action, other orders will continue. When orders in batches are cancelled, if one of the actions fails, and then jump this failed action, other orders will continue. For particular CODE, please refer to following notes |  |
```
Example：
URL: //zeus.matrix.co/v1/order/orders/batch-cancel
Above is the request URL, and below is the request parameter
{
  "clientOrderIds": "1596789621395,15967896213951,15967896213952,15967896213953,15967896213955,15967896213956,15967896213957",
  "orderIds": "1,10,20"
}
Below is the response result.
{
  "status": "success",
  "data": {
    "success": [{
      "clientOrderId": null,
      "id": "143683",
      "orderId": "143683.0"
    }, {
      "clientOrderId": null,
      "id": "143684",
      "orderId": "143684.0"
    }, {
      "clientOrderId": null,
      "id": "143685",
      "orderId": "143685.0"
    }, {
      "clientOrderId": null,
      "id": "143686",
      "orderId": "143686.0"
    }, {
      "clientOrderId": null,
      "id": "143687",
      "orderId": "143687.0"
    }, {
      "clientOrderId": null,
      "id": "143688",
      "orderId": "143688.0"
    }, {
      "clientOrderId": null,
      "id": "143689",
      "orderId": "143689.0"
    }],
    "error": []
  }
}

```

##### 7. Query order detail

API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 3 times/s

HTTP request:
· GET /v1/order/orders/{orderId}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|orderId| string | true | NA | Order id to query|

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| createdAt | true | int | Order creation time | |
| refOrderId | true | string | orders associated with this order, only works when submit cancel orders|
|userId | true | string | User Id | |
|source | true | string | Only works when the current order is cancel order| |
|symbol | true | string | currency pair corresponding to the order | |
|type | true | string | User order type |  |
|price| true | string | User order price| |
|amount| true | string | User order volume| |
|filledAmount | true | decimal | the volume of orders traded currently| |
|fee| true | string|  transaction fee | |
|triggerOn | true| string| Only works with stop-limit order, trigger price| |
|makerFeeRate | true | string | maker transaction fee ratio| |
|takerFeeRate | true | string | taker  transaction fee ratio| |
|status | true| string | current order status | |
|  orderId | true | string |  Order ID in Matrix system | |
|clientOrderId | true | string | User-defined order number(if no results, display null) | |
|feeCurrency | true | string |  currency of transaction fee| |
```
Example:
URL: //zeus.matrix.co/v1/order/orders/57171
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": {
    "createdAt": 1593683839191,
    "refOrderId": "0",
    "userId": "100196",
    "source": "",
    "symbol": "BCH_BTC",
    "type": "BUY_LIMIT",
    "price": "1.3",
    "amount": "5.5",
    "filledAmount": "5.5",
    "fee": "0.00715",
    "triggerOn": "0",
    "makerFeeRate": "0.001",
    "takerFeeRate": "0.002",
    "status": "FULLY_FILLED",
    "clientOrderId": null,
    "feeCurrency": "BTC",
    "orderId": "57171"
  }
}
```

##### 8. Search past orders

API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 3 times/s

HTTP request:
· GET /v1/order/orders

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | true | NA | Currency pair name|
| types | string | false | NA | order type queried, use ',' to query multiple query conditions|
| startTime| int | false | NA | timestamp, the beginning time of the order (no more than 48h between the end time)|
|endTime | int | false| NA | timestamp, the end time of the order(no more than 48h between the beginning time)
|status | string | false | NA | Query order status, use ',' to query multiple query conditions|
|side | string | false | NA | order side, SELL, BUY|
|from | int | false|NA | If it is next query, it is assigned as the last id in the previous query result; If i tis next query, it is assigned as the first id in the previous query result; From and direct must exist at the same time | 
|direct | string | false | NA | prev ; next |
|size | int | false | 100 | [1,100]


##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| createdAt | true | int | Order creation time | |
| refOrderId | true | string | orders associated with this order, only works when submit cancel orders|
|userId | true | string | User Id | |
|source | true | string | Only works when the current order is cancel order| |
|symbol | true | string | currency pair corresponding to the order | |
|type | true | string | User order type |  |
|price| true | string | User order price| |
|amount| true | string | User order volume| |
|filledAmount | true | string | the volume of orders traded currently| |
|fee| true | string|  transaction fee | |
|triggerOn | true| string| Only works with stop-limit order, trigger price| |
|makerFeeRate | true | string | maker transaction fee ratio| |
|takerFeeRate | true | string | taker  transaction fee ratio| |
|status | true| string | current order status | |
| orderId | true | string |  Order ID in Matrix system | |
|clientOrderId | true | string | User-defined order number(If no result, display null) | |
|feeCurrency | true | string |  currency of transaction fee| |
```
Example:
URL: //zeus.matrix.co/v1/order/orders?symbol=LTC_BTC&types=SELL_MARKET&status=FULLY_FILLED,FULLY_CANCELLED&startTime=1596769362290&endTime=1596783743947&from=134748&direct=prev&size=500&side=SELL
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": [{
    "createdAt": 1596769362290,
    "refOrderId": "0",
    "userId": "100196",
    "source": "",
    "symbol": "LTC_BTC",
    "type": "SELL_MARKET",
    "price": "0",
    "amount": "0.7",
    "filledAmount": "0.7",
    "fee": "0.00504",
    "triggerOn": "0",
    "makerFeeRate": "0.001",
    "takerFeeRate": "0.002",
    "status": "FULLY_FILLED",
    "clientOrderId": "1596769362076",
    "feeCurrency": "BTC",
    "orderId": "132894"
  }, {
    "createdAt": 1596769425057,
    "refOrderId": "0",
    "userId": "100196",
    "source": "",
    "symbol": "LTC_BTC",
    "type": "SELL_MARKET",
    "price": "0",
    "amount": "0.4",
    "filledAmount": "0.4",
    "fee": "0.00328",
    "triggerOn": "0",
    "makerFeeRate": "0.001",
    "takerFeeRate": "0.002",
    "status": "FULLY_FILLED",
    "clientOrderId": "1596769424827",
    "feeCurrency": "BTC",
    "orderId": "133233"
  }]
}
```

##### 9. Get transaction detail

API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 3 times/s

This interface is based on search condition to query and past transaction record
HTTP request:
· GET /v1/order/trade

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | true | NA | Currency pair name|
| types | string | false | NA | Query order type, use ',' to query multiple query conditions|
| startTime| int | false | NA | timestamp, the beginning time of the order(no more than 48h between the end time)|
|endTime | int | false| NA | timestamp, the end time of the order(no more than 48h than the beginning time)
|side | string | false | NA | order side: SELL, BUY|
|from | int | false|NA | If it is next query, it is assigned as the last id in the previous query result; If it is next query, it is assigned as the first; From and direct must exist at the same time | 
|direct | string | false | NA | prev ; next |
|size | int | false | 100 | [1,500]

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| tradeId | true | string | id of orders traded| |
| createdAt | true | string | order transaction time | |
| fee | true | string | transaction fee of this order|
|feeCurrency | true | string |  currency pair of transaction fee | |
|orderId | true | string | order Id| |
|price | true | string | order price | |
|side | true | string | order side，BUY or SELL |  |
|symbol| true | string | the order currency pair| |
|type| true | string | the order type| |
|userId | true | string | Matrix user ID | |
```
Example:
URL: //zeus.matrix.co/v1/order/trade?startTime=1593683955164&endTime=1593684111172&symbol=BCH_BTC&size=10&types=BUY_LIMIT,MARKET_SELL&side=BUY&from=90440&direct=prev
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": [{
    "traceId": "90398",
    "createdAt": 1593683955164,
    "fee": "0.00559",
    "feeCurrency": "BTC",
    "orderId": "57171",
    "price": "1.3",
    "side": "BUY",
    "symbol": "BCH/BTC",
    "type": "LIMIT",
    "userId": "100196",
    "amount": 4.300000000000000000
  }, {
    "traceId": "90408",
    "createdAt": 1593683973047,
    "fee": "0.00156",
    "feeCurrency": "BTC",
    "orderId": "57171",
    "price": "1.3",
    "side": "BUY",
    "symbol": "BCH/BTC",
    "type": "LIMIT",
    "userId": "100196",
    "amount": 1.200000000000000000
  }]
}
```

##### 10. Get recent transaction data

API Key permission: Read Only (Private Data)  
Rate limiting value(NEW): 5 times/s

This interface is used to search transaction information in Matrix system when search condition is currency pair.

HTTP request:
· GET /v1/order/trade/ticks/{symbol}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | true | NA | Currency pair name|

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| ------- | :----:  | :-----: | ---- | ------|
| amount | true | string | transaction volume | |
| createdAt | true | string | order transaction time | |
| direction | true | boolean | true -> Buy, false -> Sell actively| |
|price | true | string | transaction time| |
|symbol| true | string | current currency pair| |
```
Example:
URL: //zeus.matrix.co/v1/order/trade/ticks/LTC_BTC
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": [{
    "amount": "1.2",
    "createdAt": 1596785419837,
    "direction": true,
    "price": "3.8",
    "symbol": "LTC_BTC"
  }, {
    "amount": "0.1",
    "createdAt": 1596785418915,
    "direction": true,
    "price": "4.1",
    "symbol": "LTC_BTC"
  }]
}
```

##### 11. Get Candlesticks data

API Key permission: Read Only (Public Data)  
Rate limiting value(NEW): 5 times/s

This interface is used to search Candlesticks information in Matrix market, when search condition is currency pair.

HTTP request:
· GET /v1/order/trade/bars/{symbol}/{type}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | true | NA | Currency pair name|
|type| string | true | NA | Candlesticks type | 

##### Note:
Candlesticks type include: K_1_MIN, K_5_MIN, K_15_MIN, K_30_MIN, K_1_HOUR, K_4_HOUR, K_8_HOUR, K_12_HOUR, K_1_DAY, K_1_WEEK

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
|---| --- | --- | --- |---|
|bars | true | string[] | represent respectively: [timestamp of Candlesticks, the opening price, the high price,the closed price, the low price, transaction volume] | |
```
Example:
URL: //zeus.matrix.co/v1/order/trade/bars/ETH_BTC/K_1_WEEK
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": ["[1594569600000,1.5,3.3,1.4,2.5,6.4163]", "[1595779200000,1.6,3.3,1.6,3.3,28.0]", "[1596384000000,3.3,3.4,1.4,3.1,318.5]"]
}
```

##### 12. Get the transaction fee ratio of current user

API Key permission: Read Only (Public Data)  
Rate limiting value(NEW): 5 times/s

HTTP request:
· GET /v1/order/trade/fee

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | true | NA | Currency pair name|

##### Response Content:
| Parameter Name | Required | Data Type | Description | Value Range |
| --- | --- | --- | --- | --- |
|taker | true | string | TAKER  transaction fee ratio | |
|maker | true | string | MAKER  transaction fee ratio | |
```
Example:
URL: //zeus.matrix.co/v1/order/trade/fee?symbol=BTC_USD
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": {
    "maker": 0.001,
    "taker": 0.002
  }
}
```

##### 13. Get market price of currency pair

API Key permission: Read Only (Public Data)  
Rate limiting value(NEW): 5 times/s

HTTP request:
· GET /v1/order/trade/prices

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | false | NA | Currency pair name|

##### Response Content:
| key | value |
| -- | -- |
| currency pair information | [timestamp, the opening price, the high price, the closed price, the low price, transaction volume]
```
Example:
URL: //zeus.matrix.co/v1/order/trade/prices?symbol=LTC_BTC
Above is the request URL, and below is the response result
{
  "status": "success",
  "data": {
    "LTC_BTC": "[1596698603073,12.0,12.0,3.3,3.4,203.764,242.0]"
  }
}
```

##### 14. Get Currency active list and Symbol sctive list

API Key permission: Read Only (Public Data)  
Rate limiting value(NEW): 5 times/s

HTTP request:
· GET /v1/market/trades
##### Request parameter:
None  

##### Response Content:
|Set name| Parameter | Data Type | Required | Default | Description|
| --- |---| --- | --- | --- | --- |
|currencies|  | list | true | NA | |
| | name | string | true | NA | Currency name | 
| | scale | int | true | NA | currency precision | 
| | tokenDecimals | int | true | NA | |
| symbols| | list | true | NA | |
| | name | string | true | NA | Currency pair name |
| | baseName | string | true | NA | Base currency name|
| | baseScale | int | true | NA | Base currency precision|
| | baseMinimum | string | true | NA | Base currency minimum precision | 
| | quoteName | string | true| NA | Quote currency name|
| | quoteScale | int | true | NA | Quote currency precision | 
| | quoteMinimum | string |  true | NA | Quote currency minimumu precision|

```
Example:
URL: https://zeus.matrix.co/v1/market/trades
Above is the request URL, and below is the response result.
{
  "status": "success",
  "data": {
    "symbol": [{
      "name": "BCH_BTC",
      "baseName": "BCH",
      "baseScale": 4,
      "baseMinimum": "0.0005",
      "quoteName": "BTC",
      "quoteScale": 6,
      "quoteMinimum": "0.00001"
    }, {
      "name": "BCH_ETH",
      "baseName": "BCH",
      "baseScale": 4,
      "baseMinimum": "0.0005",
      "quoteName": "ETH",
      "quoteScale": 8,
      "quoteMinimum": "0.00000001"
    }],
    "currency": [{
      "name": "BCH",
      "scale": 4,
      "tokenDecimals": 8
    }, {
      "name": "BTC",
      "scale": 6,
      "tokenDecimals": 8
    }]
  }
}

```

## WEBSOCKET Description

### Access URL
Matrix market data request address

wss://zeus-wss.matrix.co

### Permission
Read Only (Public Data)  

#### Heartbeat information
When user's Websocket client server is connected to the Websocket server of Matrix, the server will periodically (currently set 5s interval) send ping message including a following integer:

{"ping": 1492420473027}

When user's Websocket client server recevies the Heartbeat information, it shoud return pong message and include the same integer value:

{"pong": 1492420473027}

<strong>When the time interval that the client server cannot receive the Pong notification from the server is greater than 30s, the server will disconnect and reconnect the client.</strong>

#### Subscribe to Topic
After successfully establishing the connection to Websocket server, Websocket client server will send following request to subscribe particular topic:

{"action":"subscribe","symbol":"$symbol"}  

<strong>$symbol represent the currency pair information that user need to subscribe</strong>

After successful subscription, Websocket client server will receive the confirmation:

{"status":"connected as anonymous user"}
{"status":"subscribed"}

After that, once the topic subscribed updates, Websocket client server will receive the updated message pushed by the server.

Take the symbol LTC_BTC as an example. The updated message will be displayed in the form of :

["topic_price",{"LTC_BTC":"[1596678861770,3.5,11.9,3.1,4.9,135650.61599999998,274308.0]"}]


##### Market Candlesticks data
###### Subscribe to Topic

Once Candlesticks data is generated, Websocket server will push message to client server through this topic interface subscribed:

{"action":"subscribe_bar","symbol":"$symbol"}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | false | NA | Currency pair name|

##### Update Content
| Field | Data Type | Description | 
| --- | --- | --- |
| symbol | string | Currency pair name | 
| type | string | Candlesticks type | 
| data | object[] | Correspond respectively to [timestamp, the opening price, the high price, the closed price, the low price, transaction volume]|
```
["topic_bar", {
  "symbol": "LTC_BTC",
  "type": "K_1_WEEK",
  "data": [1596384000000, 3.5, 11.9, 3.1, 3.7, 135713.866]
}]
```

##### Market currency pair depth information
###### Subscribe to Topic

Once Candlesticks data is generated, Websocket server will push message to client server through this topic interface subscribed:

{"action":"subscribe_snapshot","symbol":"$symbol"}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | false | NA | Currency pair name|

##### Update Content
| Field | Data Type | Description | 
| --- | --- | --- |
| buyOrders | list | Market depth buy order | 
| price | string | Current market price | 
| sellOrders | list | Market depth sell order|
| symbol | string | Market currency pair | 
| timestamp | string | Server sending time | 
```
["topic_snapshot", {
  "symbol": "LTC_BTC",
  "sequenceId": 137356,
  "timestamp": 1596788101270,
  "price": 4.200000000000000000,
  "buyOrders": [{
    "price": 3.700000000000000000,
    "amount": 0.300000000000000000
  }, {
    "price": 3.500000000000000000,
    "amount": 2.400000000000000000
  }],
  "sellOrders": [{
    "price": 4.100000000000000000,
    "amount": 0.500000000000000000
  }, {
    "price": 4.400000000000000000,
    "amount": 0.346000000000000000
  }, {
    "price": 4.500000000000000000,
    "amount": 16.068000000000000000
  }]
}]

```

##### Market transaction information
###### Subscribe to Topic

Once Candlesticks data is generated, Websocket server will push message to client server through this topic interface subscribed:

{"action":"subscribe_tick","symbol":"$symbol"}

##### Request parameter:
| Parameter | Data Type | Required | Default | Description|
|---| --- | --- | --- | --- |
|symbol| string | false | NA | Currency pair name|

##### Update Content
| Field | Data Type | Description | 
| --- | --- | --- |
| amount | string | transaction volume | 
| createdAt | int | Engine transaction time | 
| direction | boolean | false represents sell actively，true represents buy actively|
| price | string | Market price | 
| symbol | string | Current currency pair | 
```
["topic_tick", {
  "symbol": "LTC_BTC",
  "price": 4.500000,
  "amount": 0.001,
  "direction": true,
  "createdAt": 1596788810935,
  "takerUserId": 0,
  "makerUserId": 0
}]
```

## API Summary
| API Type | API Name | Access Permission | Operation Control | Remark|
|---| --- | --- | --- | --- |
|REST API| Information of account balance | API KEY | Read Only | |
|REST API| Information of account balance (single currency) | API KEY | Read Only | |
|REST API| Place order | API KEY | Trade | |
|REST API| Place a batch of orders | API KEY | Trade | |
|REST API| Submit cancel for an order | API KEY | Trade | |
|REST API| Submit cancel for a batch of orders | API KEY | Trade | |
|REST API| Query order detail | API KEY | Read Only | |
|REST API| Search past orders | API KEY | Read Only | |
|REST API| Get transaction detail | API KEY | Read Only | |
|REST API| Get recent transaction data | API KEY | Read Only | |
|REST API| Get Candlesticks data | API KEY | Read Only | Public Data |
|REST API| Get the transaction fee ratio of current user | API KEY | Read Only | Public Data |
|REST API| Get market price of currency pair | API KEY | Read Only | Public Data |
|REST API| Get Currency active list and Symbol sctive list | API KEY | Read Only | Public Data |
|WebSocket| Information of account balance | Public | Read Only | Public Data |
