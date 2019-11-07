# Perpetual API

永续合约交易的行情信息，账户信息，订单操作，订单查询，账单明细查询

## Position

### Get all positions infomation



获取账户所有合约的持仓信息。请求此接口，会在数据库遍历所有币对下的持仓数据，有大量的性能消耗,请求频率较低。建议用户传合约ID获取持仓信息。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v3/position`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| instrument_id | String   | Yes       | Instrument ID，for example: BTC_USD |

#### Response Parameters

>Response

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| instrument_id | String   | Instrument ID，for example: BTC_USD |





### Get Position Info

获取某个合约的持仓信息


#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/order/holdPositionList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | 否       | 产品编号，如ETH/USDT |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "id": 1,
            "productCode": "ETH/USDT",
            "orderSide": 2,
            "marginAmount": 1,
            "averagePrice": 2,
            "quantity": 2,
            "flatQuantity": 2,
            "frozenQuantity": 2,
            "currentPrice": 2,
            "winLoss": 2,
            "winLossRate": 2
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| id | long   | 持仓编号 |
| productCode | string   | 产品编号 |
| orderSide | string   | 订单方向 |
| marginAmount | double   | 保证金 |
| averagePrice | double   | 持仓均价 |
| quantity | double   | 持仓数量 |
| flatQuantity | double   | 可平数量（持仓数量-冻结数量） |
| frozenQuantity | double   | 冻结数量 |
| currentPrice | double   | 公允价格 |
| winLoss | double   | 盈亏 |
| winLossRate | double   | 盈亏率 |

## Account


### 账户资产信息

获取账户资产信息


#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/account/info`

#### Request Parameters

无

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": {
        "coinCode": "USDT",
        "availableAmount": 1000000.00000000,
        "marginAmount": 0E-8,
        "frozenAmount": 0E-8,
        "winLoss": 0,
        "riskRatio":0
    }
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| coinCode | string   | 币种code |
| availableAmount | double   | 可用金额 |
| marginAmount | double   | 已用金额 |
| frozenAmount | double   | 冻结金额 |
| winLoss | double   | 浮动盈亏 |
| riskRatio | double   | 风险率 |

### 单个币种合约账户信息

获取单个币种合约的账户信息

### 获取某个合约的用户配置

获取某个合约的杠杆倍数，持仓模式

### 设定某个合约的杠杆

设定某个合约的杠杆倍数

### 账户资产流水查询

列出账户资产流水，账户资产流水是指导致账户余额增加或减少的行为。流水会分页，每页100条数据，并且按照时间倒序排序和存储，最新的排在最前面。 本接口能查询最近7天的数据。


#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/account/transactionList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pageSize | int   | 否       | 页大小，默认为20 |
| pageNumber | int   | 否       | 当前页，默认为0 |
| transactionType | int   | 否       | 类型，默认为0 |
| beginTime | long   | 否       | 开始时间 |
| endTime | long   | 否       | 结束时间 |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": {
        "coinCode": "USDT",
        "transactionType": 1,
        "amount": 0E-8,
        "createTime":1846465984469
    }
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| coinCode | string   | 币种code |
| transactionType | int   | 类型 |
| amount | double   | 金额 |
| createTime | long   | 创建时间 |

## 订单

### 下单

API交易提供限价单下单模式，只有当您的账户有足够的资金才能下单。一旦下单，您的账户资金将在订单生命周期内被冻结，被冻结的资金以及数量取决于订单指定的类型和参数。目前API下单只支持以USDT为计价单位



#### Rate Limit

5 requests per second

#### HTTP Request

`POST /api/v1/cfd/order/create`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| price | double   | Yes       | 价格 |
| total | double   | Yes       | 数量 |
| operationType | int   | Yes       | 操作类型 |
| source | int   | Yes       | 订单来源 |
| orderSide | int   | Yes       | 订单方向 |
| productCode | string   | Yes       | 产品code |
| orderType | int   | Yes       | 订单类型 |
| marginRatio | double   | Yes       | 保证金比例 |


#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": {
        "id": 2613492510000000001,
        "price": 3.1,
        "total": 10.00000000,
        "orderType": 1,
        "operationType": 0,
        "orderSide": 1,
        "fee": 0,
        "averagePrice": 0,
        "filled": 0,
        "marginRatio": 0.75,
        "frozenMargin": 23.25000000,
        "createTime": 1568095542365
    }
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| id | long   | 订单ID |
| price | double   |  价格 |
| total | double   |  数量 |
| operationType | int   | 操作类型 |
| orderSide | int   |  订单方向 |
| productCode | string   |  产品code |
| orderType | int   |  订单类型 |
| averagePrice | double   |  成交均价 |
| fee | double   |  手续费 |
| marginRatio | double   | 保证金比例 |
| frozenMargin | double   |  冻结保证金 |
| createTime | long   |  下单时间 |



### 批量下单

批量进行下单请求，每个合约可批量下10个单。

### 撤单

撤销之前下的未完成订单。



#### Rate Limit

5 requests per second

#### HTTP Request

`POST /api/v1/cfd/order/cancel`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| orderId | long   | Yes       | 订单编号 |



#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success"
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |




### 批量撤单

撤销之前下的未完成订单，每个币对可批量撤10个单。

### 获取所有订单列表

列出您当前所有的订单信息。本接口能查询最近7天20000条数据。这个请求支持分页，并且按委托时间倒序排序和存储，最新的排在最前面。

### 获取订单信息

通过订单id获取单个订单信息。已撤销的未成交单只保留2个小时。

## 交易

### 获取成交明细

获取最近的成交明细列表，本接口能查询最近7天的数据。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/order/closePositionList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | 否       | 产品编号，如ETH/USDT |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "BTC_USDT_1",
            "orderSide": 1,
            "lastAveragePrice": 10059.43451275,
            "quantityChange": 15.00000000,
            "averagePrice":1.1,
            "winLossRate":0.1,
            "dealPrice": 0,
            "fee": 50.18500000,
            "winLoss": -22.43451275,
            "createTime": 1568966027808
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| productCode | string   | 产品编号 |
| orderSide | int   | 订单方向详情 |
| averagePrice | double   | 成交均价 |
| lastAveragePrice | double   | 持仓均价 |
| quantityChange | double   | 平仓数量 |
| dealPrice | double   | 平仓价 |
| winLoss | double   | 盈亏 |
| winLossRate | double   | 盈亏率 |
| createTime | long   | 平仓时间 |


### 获取委托单列表

获取委托单列表: 列出您当前所有的订单信息。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/order/currentOrderList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | 否       | 产品编号，如ETH/USDT |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "id": 1,
            "productCode": "ETH/USDT",
            "orderSide": 2,
            "price": 1,
            "averagePrice": 2,
            "frozenMargin": 2,
            "filled": 2,
            "marginRatio": 2,
            "fee": 2,
            "createTime": 1568611851508,
            "operationType":1,
            "total":11，
            "orderStatus":10
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| id | long   | 订单编号 |
| productCode | string   | 产品编号 |
| operationType | int   | 操作类型 |
| orderSide | int   | 订单方向详情 |
| total | double   | 委托数量 |
| price | double   | 委托价格 |
| averagePrice | double   | 成交均价 |
| frozenMargin | double   | 冻结保证金 |
| filled | double   | 成交数量 |
| marginRatio | double   | 保证金比例 |
| fee | double   | 手续费 |
| createTime | long   | 委托时间 |
| orderStatus | int   | 订单状态详情 |

### 获取历史委托单列表

获取历史委托单列表: 列出您历史委托单信息。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/order/historyOrderList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | 否       | 产品编号，如ETH/USDT |
| pageSize | int   | 否       | 页大小，默认为20 |
| pageNumber | int   | 否       | 当前页，默认为0 |
| beginTime | long   | 否       | 开始时间 默认查全部，不为空时，后端自动向前推24小时 |
| endTime | long   | 否       | 结束时间 默认查全部，不为空时，后端自动向前推24小时 |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "btc_usdt_1",
            "orderSide": 1,
            "price": 10006.000,
            "averagePrice": 0,
            "filled": 0,
            "marginRatio": 0,
            "fee": 0,
            "createTime": 1568779561000
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| orderId | long   | 订单编号 |
| productCode | string   | 产品编号 |
| orderSide | int   | 订单方向详情 |
| price | double   | 委托价格 |
| averagePrice | double   | 成交均价 |
| frozenMargin | double   | 冻结保证金 |
| filled | double   | 成交数量 |
| marginRatio | double   | 保证金比例 |
| fee | double   | 手续费 |
| createTime | long   | 委托时间 |

### 获取历史平仓列表

获取历史平仓列表: 列出您历史平仓信息。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/order/historyPositionList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | 否       | 产品编号，如ETH/USDT |
| pageSize | int   | 否       | 页大小，默认为20 |
| pageNumber | int   | 否       | 当前页，默认为0 |
| orderSide | int   | 否       | 买卖方向 |
| beginTime | long   | 否       | 开始时间 默认查全部，不为空时，后端自动向前推24小时 |
| endTime | long   | 否       | 结束时间 默认查全部，不为空时，后端自动向前推24小时 |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "btc_usdt_1",
            "orderSide": 2,
            "marginAmount": 805.29060361,
            "averagePrice": 10066.13254486,
            "quantity": 8.00000000,
            "fee": 50.49500000,
            "dealPrice":12.4,
            "winLoss": -32.86745515,
            "createTime":124946498634
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| productCode | string   | 产品编号 |
| orderSide | int   | 订单方向详情 |
| marginAmount | double   | 保证金 |
| averagePrice | double   | 成交均价 |
| quantity | double   | 平仓数量 |
| dealPrice | double   | 平仓价 |
| winLoss | double   | 盈亏 |
| createTime | long   | 平仓时间 |

### 获取历史成交列表

获取历史成交列表: 列出您历史成交信息。



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/trade/list`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | string   | Yes       | 产品编号，如ETH/USDT |
| size | int   | 否       | 列表长度，默认为20 |


#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "BTC/USDT",
            "orderSide": 2,
            "time": 1570689717187,
            "price": 10033.000,
            "qty":12
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| productCode | string   | 产品编号 |
| orderSide | int   | 订单方向详情 |
| price | double   | 成交较 |
| qty | double   | 成交数量 |
| time | long   | 成交时间 |


### 链类型列表



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/wallet/chainTypeList`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | String   | Yes       | 产品code |
| size | int   | Yes       | 深度的数量 |


#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "chinType": 1,
            "coinCode": "USDT",
            "feeRate": 0.1,
            "chainTypeRemark": "OMNI"
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| id | long   | 产品编号 |
| productCode | String   | 产品code |
| price | double   | 价格 |
| qty | double   | 数量 |

### 获取深度数据



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/depth/get`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | String   | Yes       | 产品code |
| size | int   | Yes       | 深度的数量 |

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": {
        "productCode": "BTC/USDT",
        "sell": [
            {
                "price": 10048.000,
                "qty": 2.00000000
            }
        ],
       "buy":[
            {
                "price": 10048.000,
                "qty": 2.00000000
            }
        ]
    }
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| id | long   | 产品编号 |
| productCode | String   | 产品code |
| price | double   | 价格 |
| qty | double   | 数量 |

## Market Info

### 获取行情数据



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api/v1/cfd/kline/get`

#### Request Parameters

无

#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "BTC/USDT",
            "periodType": "1T",
            "timestamp": 1568958840000,
            "open": 10097.000,
            "high": 10099.000,
            "low": 10000.000,
            "close": 10039.000,
            "volume": 4831.00000000,
            "amount": 48544204.00000000,
            "lastClose": 10033.000,
            "changeAmount": 6.000,
            "changeRatio": 0.0006,
            "basePrice": 10169.339
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| productCode | string   | 产品code |
| timestamp | long   | 时间 |
| open | double   | 开盘价 |
| high | double   | 最高价 |
| low | double   | 最低价 |
| close | double   | 关闭价 |
| volume | double   | 成交数量 |
| amount | double   | 成交金额 |
| change | double   | 与上一条的变更 |
| lastClose | double   | 最新成交价格 |
| basePrice | double   | 外部价格 |
| positionSum | double   | 持仓 |


### 获取K线数据



#### Rate Limit

5 requests per second

#### HTTP Request

`GET /api//v1/cfd/market/get`

#### Request Parameters

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| productCode | String   | Yes       | 产品code |
| size | int   | 否       | 深度的数量 |
| periodType | String   | Yes       | k线周期类型 |
| beginTime | long   | 否       | 开始时间 |


#### Response Parameters

>Response

```json
{
    "success": 0,
    "message": "success",
    "data": [
        {
            "productCode": "BTC_USDT_1",
            "open": 10012.000,
            "high": 10099.000,
            "low": 10000.000,
            "close": 10012.000,
            "volume": 778727.00000000,
            "amount": 7824750996.00000000,
            "lastPrice": 10012.000,
            "change": 0.00,
            "timestamp": 1568960052830,
            "basePrice": 10183.668,
            "positionSum":1
        }
    ]
}
```

| Parameters        | Type |         Description         |
|---------------|----------|--------------------|
| success | int   | 接口返回状态，0为成功 |
| message | string   | 接口返回描述 |
| productCode | string   | 产品code |
| periodType | string   | 周期类型 |
| timestamp | long   | 时间 |
| open | double   | 开盘价 |
| high | double   | 最高价 |
| low | double   | 最低价 |
| close | double   | 关闭价 |
| volume | double   | 成交数量 |
| amount | double   | 成交金额 |
| lastClose | double   | 最新成交价格 |
| changeAmount | double   | 变更数量 |
| changeRatio | double   | 变量率 |
| basePrice | double   | 外部价格 |
