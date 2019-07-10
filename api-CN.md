# REST行情与交易接口 (2019-07-08)
# 基本信息
* 本篇列出REST接口的baseurl **https://api.matrix.cn**
* 所有接口的响应都是JSON格式
* 响应中如有数组，数组元素以时间升序排列，越早的数据越提前。
* 所有时间、时间戳均为UNIX时间，单位为毫秒
# 接入说明
## 签名认证
### 签名说明
API 请求在通过 internet 传输的过程中极有可能被篡改，为了确保请求未被更改，除公共接口（基础信息，行情数据）外的私有接口均必须使用您的 API Key 做签名认证，以校验参数或参数值在传输途中是否发生了更改。每一个API Key需要有适当的权限才能访问相应的接口。每个新创建的API Key都需要分配权限。权限类型分为：读取，交易，提币。在使用接口前，请查看每个接口的权限类型，并确认你的API Key有相应的权限。

一个合法的请求由以下几部分组成：
* 方法请求地址：即访问服务器地址 api.matrix.cn，比如 api.matrix.cn/v1/trade/orders。
* API 访问密钥（API-KEY）：您申请的 API Key 中的 API Key。
* 签名方法（API-SIGNATURE-METHOD）：用户计算签名的基于哈希的协议，此处使用 HmacSHA256。
* 签名版本（API-SIGNATURE-VERSION）：签名协议的版本，此处使用1。
* 时间戳（API-TIMESTAMP）：您发出请求的时间 (UTC 时区) (UTC 时区) (UTC 时区) 。如：2017-05-11T16:22:06。在查询请求中包含此值有助于防止第三方截取您的请求。
* 必选和可选参数：每个方法都有一组用于定义 API 调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。 请一定注意：对于 GET 请求，每个方法自带的参数都需要进行签名运算； 对于 POST 请求，每个方法自带的参数不进行签名认证，即 POST 请求中需要进行签名运算的只有 AccessKeyId、SignatureMethod、SignatureVersion、Timestamp 四个参数，其它参数放在 body 中。
* 签名：签名计算得出的值，用于确保签名有效和未被篡改。

# 公开API接口
## 行情接口

### K线图数据

**Path：** /v1/market/bars/{symbol}/{type}

**Method：** GET

### 请求参数

**路径参数：**

| 参数名称 | 示例  | 备注  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |  交易对 |
| type |  K_1_HOUR |  K线图的数据类型 |
**参数：**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| end | 否  |  1552176000000 |  结束时间 |
| start | 否  |  1552100000000 |  开始时间 |

### 响应：

```
{
   “ lastUpdateId ”： 1027024，
   “出价”： [
    [
      “ 4.00000000 ”，      //价格
      “ 431.00000000 ”     //数量
    ]
  ]
  “问”： [
    [
      “ 4.00000200 ”，
       “ 12.00000000 ”
    ]
  ]
}
```

### 24hr价格变动情况

**Path：** /v1/market/prices

**Method：** GET

**接口描述：**

<p>逻辑是:<br>
&nbsp;&nbsp; price = bar[4]; // 价格<br>
&nbsp;&nbsp; change = (bar[4] - bar[1]) / bar[1]; // 24小时变化量<br>
&nbsp;&nbsp; amount = bar[5]; // 交易量<br>
&nbsp;&nbsp; direction = change &gt;= 0; // 涨还是跌</p>

### 请求参数

### 响应：

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

### 最新挂单价格

**Path：** /v1/market/snapshot/{symbol}

**Method：** GET

**接口描述：**

### 请求参数

**路径参数**

| 参数名称 | 示例  | 备注  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |  交易对 |

### 响应：

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

### 最新成交价格

**Path：** /v1/market/ticks/{symbol}

**Method：** GET

**接口描述：**

**路径参数**

| 参数名称 | 示例  | 备注  |
| ------------ | ------------ | ------------ |
| symbol | BCH_BTC |  交易对 |
**参数**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| num | 否  |   |  默认50条数据 |

### 响应：

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

## 币种和交易对信息查询

**Path：** /v1/market/trades

**Method：** GET

**接口描述：**


### 请求参数

**响应：**

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
## 账户接口 

##### 访问账户相关的接口需要进行签名认证。

### 账户信息 

**API Key 权限：读取**

**Path：** /v1/user/accounts 

**Method：** GET

**接口描述：**

### 请求参数

**响应：**

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
            },
            {
                "currency": "ETC",
                "available": "123600000.000000000000000000",
                "frozen": "0",
                "locked": "0",
                "estimate": "957495118.40",
                "legal": false
            },
            {
                "currency": "BCH",
                "available": "123599998.208600000000000000",
                "frozen": "0.000000000000000000",
                "locked": "0",
                "estimate": "51049065838.67",
                "legal": false
            },
            {
                "currency": "USD",
                "available": "123600000.000000000000000000",
                "frozen": "0",
                "locked": "0",
                "estimate": "0.00",
                "legal": true
            },
            {
                "currency": "ETH",
                "available": "123599991.320700000000000000",
                "frozen": "9.018000000000000000",
                "locked": "0",
                "estimate": "36418709180.31",
                "legal": false
            },
            {
                "currency": "LTC",
                "available": "123600000.422900000000000000",
                "frozen": "0",
                "locked": "0",
                "estimate": "14780092366.35",
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

## 交易 
##### 访问账户相关的接口需要进行签名认证

### 下单 
**API Key 权限：交易**

**Path：** /v1/trade/orders (HMAC SHA256)

**Method：** POST

**接口描述：** 创建新订单



#### 请求参数

**参数**

| 参数名称 | 是否必须 | 类型   | 描述                                                         | 默认值 | 取值范围                                                     |
| -------- | -------- | ------ | ------------------------------------------------------------ | ------ | ------------------------------------------------------------ |
| amount   | true     | string | 限价单表示下单数量，市价买单时表示买多少钱（此时最大精度固定为8），市价卖单时表示卖多少币 |        |                                                              |
| price    | false    | string | 下单价格，市价单不传该参数                                   |        |                                                              |
| source   | false    | string | 订单来源                                                     |        |                                                              |
| symbol   | true     | string | 交易对                                                       |        | BTC_USDT, ETH_BTC...                                         |
| type     | true     | string | 订单类型                                                     |        | BUY_MARKET：市价买, SELL_MARKET：市价卖, BUY_LIMIT：限价买,SELL_LIMIT：限价卖 |
|          |          |        |                                                              |        |                                                              |

#### 响应:

```
{
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
### 撤销订单 

**API Key 权限：交易**

**Path：** /v1/trade/orders/{orderId}/cancel (HMAC SHA256)

**Method：** POST

**接口描述：** 取消订单

#### 请求参数

**路径参数**

| 参数名称 | 示例   | 备注   |
| -------- | ------ | ------ |
| orderId  | 100006 | 订单ID |



响应

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

### 查询订单 

**API Key 权限：读取**

**Path：** /v1/trade/orders/{orderId} 

**Method：** GET

**接口描述：**


### 请求参数
**路径参数**

| 参数名称 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| orderId |  100003 |  订单id |

### 响应:

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

### 查询订单的成交明细 

**API Key 权限：读取**

**Path：** /v1/trade/orders/{orderId}/matches  (HMAC SHA256)

**Method：** GET

**接口描述：**


### 请求参数
**路径参数**

| 参数名称 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| orderId |  100009 |  存在对应交易的订单id |
**参数**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| limit | 否  |  10 |  limit |
| offsetId | 否  |  0 |  offsetId |

### 响应

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

### 查询所有订单

**API Key 权限：读取**

**Path：** /v1/trade/orders  (HMAC SHA256) 

**Method：** GET

**接口描述：**


### 请求参数

**参数**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| limit | 否  |  10 |  limit |
| offsetId | 否  |  0 |  offsetId |
| symbol | 否  |  ETH_USDT |  symbol |

### 响应:

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

### 查询订单

**API Key 权限：读取**

**Path：** /v1/trade/orders-by-pagination  (HMAC SHA256) 

**Method：** GET

**接口描述：**


### 请求参数
**Headers**

| 参数名称  | 参数值  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| Authorization  |   | 是  |  AAAADAVZ010000016a06ec814fWEB5e94141d3dd4e015bc2bb7623dde28fdb9fe0acc6f04faa6006784eef3e7b79f |  用户的有效Token |
**参数**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pageNumber | 否  |  10 |  当前页码 |
| pageSize | 否  |  20 |  一页显示条数 |
| symbol | 否  |  ETH_USDT |  symbol |
| start | 否  |  2019-04-23 |  开始时间 |
| end | 否  |  2019-05-23 |  结束时间 |
| orderType | 否  |  SELL |  买卖单标识 |

### 响应

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

### 查询订单明细

**API Key 权限：读取**

**Path：** /v1/trade/matchDetails-by-pagination

**Method：** GET

**接口描述：**


### 请求参数

**参数**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pageNumber | 否  |  10 |   |
| pageSize | 否  |  20 |   |
| symbol | 否  |  ETH_USDT |   |
| start | 否  |  2019-04-23 |   |
| end | 否  |  2019-05-23 |   |
| orderType | 否  |  SELL |   |

### 响应

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





