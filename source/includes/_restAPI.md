# 币币 REST API 

## 私有接口

### 获取帐户信息

#### 请求

`POST https://openapi.idax.pro/api/v2/userinfo`


#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| key	| String	| 是	| 用户申请的key |
| sign	| String	| 是	| 请求参数的签名 |
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "total": {
        "BTC": "0",
            "ETH": "0"
        },
    "free": {
        "BTC": "0",
        "ETH": "0"
        },
    "freezed": {
        "BTC": "0",
        "ETH": "0"
        }
}
```

| 参数名         |         描述         |
|---------------|:--------------------:|
| total	| 总资产(ms) |
| free	| 可用 |
| freezed	| 冻结 |

### 下单

#### 请求

`POST https://openapi.idax.pro/api/v2/placeOrder`

<aside class="notice">

除此接口外，其他接口均采用quantity表示"量"，amount表示"额"

</aside>


#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	 | 是	  | IDAX支持的交易对 |
| orderType	| String	 | 是	  | limit:限价，market:市价 |
| orderSide	| String	 | 是	  | buy:买，sell:卖 |
| price	| Double	 | 是	  | 下单价格 |
| amount	| Double	 | 是	  | 下单数量 |
| triggerPrice	| Double	 | 否	  | 触发价格（如果是非标准单，触发价必须大于0） |
| priceGap	| Double	 | 否	  | 回调差价（如果是移动止损，回调价格必须大于0）（限价止损、止盈不支持回调价差） |
| orderMode	| Integer	 | 是	  | 订单模式(普通:0; 止损:10; 止盈:11; 移动止损:20) |
| orderProperty	| Integer	 | 是	  | 订单属性:orderProperty:(普通:0; FAK:1; FOK:2; 摆盘:3) |
| key	| String	 | 是	  | 用户申请的key |
| sign	| String	 | 是	  | 请求参数的签名 |
| timestamp	| Long	 | 是	  | 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success", 
    "orderId":"2000000000008832432"
}
```

| 参数名         |         描述         |
|---------------|:--------------------:|
| orderId	| 订单ID | 

### 撤单

#### 请求

`POST https://openapi.idax.pro/api/v2/cancelOrder`

<aside class="notice">

支持批量撤单

</aside>


#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| orderId	| String	| 是 |	订单ID(多个订单ID中间以","分隔,一次最多允许撤消5个订单) |
| key	| String	| 是 |	用户申请的key |
| sign	| String	| 是 |	请求参数的签名 |
| timestamp	| Long	| 是 |	请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "accepted":"123456789"
}
```

| 参数名         |         描述         |
|---------------|:--------------------:|
| accepted	| 订单号(已接受取消订单请求的最后一个订单号) |

### 获取订单信息

#### 请求

`POST https://openapi.idax.pro/api/v2/orderInfo`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| orderId	| String	| 是	| 订单ID.-1:所有未完成订单；否则查询指定的订单 |
| pageIndex	| int	| 否	| 当前页 |
| pageSize	| int	| 否	| 一页显示多少条 |
| key	| String	| 是	| 用户申请的key |
| sign	| String	| 是	| 请求参数的签名 |
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "orders": [
        {
        "avgPrice": "string",
        "dealQuantity": "string",
        "orderId": 0,
        "orderMode": 0,
        "orderProperty": 0,
        "orderSide": "string",
        "orderState": 0,
        "price": "string",
        "priceGap": 0,
        "quantity": "string",
        "timestamp": 0,
        "triggerPrice": 0
        }
    ],
        "total": 1
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| orders	| 订单信息 |
| quantity	| 委托数量 |
| avgPrice	| 平均成交价 |
| timestamp	| 委托时间 |
| dealQuantity	| 成交数量 |
| orderId	| 订单ID |
| price	| 委托价格 |
| orderState	| 1:未成交,2:部分成交,9:完全成交,19:已撤单 |
| orderSide	| 1:买 / 2:卖 |
| triggerPrice	| 触发价 |
| priceGap	| 回调差价 |
| orderMode	| 订单模式(普通:0; 止损:10; 止盈:11; 移动止损:20) |
| orderProperty	| 订单属性:orderProperty:(普通:0; FAK:1; FOK:2; 摆盘:3) |
| total	| 总记录数 |

### 获取订单列表

#### 请求

`POST https://openapi.idax.pro/api/v2/orderList`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| orderId	| String	| 是	| 订单ID(多个订单ID中间以","分隔,一次最多允许查询50个订单) |
| pair	| String	| 是	| IDAX支持的交易对 |
| pageIndex	| int	| 否	| 当前页 |
| pageSize	| int	| 否	| 一页显示多少条 |
| key	| String	| 是	| 用户申请的key |
| sign	| String	| 是	| 请求参数的签名 |
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "orders": [
        {
        "avgPrice": "string",
        "dealQuantity": "string",
        "orderId": 0,
        "orderMode": 0,
        "orderProperty": 0,
        "orderSide": "string",
        "orderState": 0,
        "price": "string",
        "priceGap": 0,
        "quantity": "string",
        "timestamp": 0,
        "triggerPrice": 0
        }
    ],
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| orders	| 订单信息 |
| quantity	| 委托数量 |
| avgPrice	| 平均成交价 |
| timestamp	| 委托时间 |
| dealQuantity	| 成交数量 |
| orderId	| 订单ID |
| price	| 委托价格 |
| orderState	| 1:未成交,2:部分成交,9:完全成交,19:已撤单 |
| orderSide	| 1:买 / 2:卖 |
| triggerPrice	| 触发价 |
| priceGap	| 回调差价 |
| orderMode	| 订单模式(普通:0; 止损:10; 止盈:11; 移动止损:20) |
| orderProperty	| 订单属性:orderProperty:(普通:0; FAK:1; FOK:2; 摆盘:3) |

### 获取24小时内的订单信息

#### 请求

`POST https://openapi.idax.pro/api/v2/orderHistory`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| orderState	| Integer	| 是	| 查询状态。-1:所有状态, 0：未完成状态, 1：已完成状态。（只返回最近两天的数据） | 
| pair	| String	| 是	| IDAX支持的交易对 | 
| pageIndex	| Integer	| 否	| 当前页数 | 
| pageSize	| Integer	| 否	| 每页条数，最多1000条 | 
| key	| String	| 是	| 用户申请的key | 
| sign	| String	| 是	| 请求参数的签名 | 
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） | 

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "orders": [
        {
        "avgPrice": "string",
        "dealQuantity": "string",
        "orderId": 0,
        "orderMode": 0,
        "orderProperty": 0,
        "orderSide": "string",
        "orderState": 0,
        "price": "string",
        "priceGap": 0,
        "quantity": "string",
        "timestamp": 0,
        "triggerPrice": 0
        }
    ],
    "pageIndex": 1,
    "pageSize": 1,
    "total": 3
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| orders	| 订单信息 | 
| quantity	| 委托数量 | 
| avgPrice	| 平均成交价 | 
| timestamp	| 委托时间 | 
| dealQuantity	| 成交数量 | 
| orderId	| 订单ID | 
| price	| 委托价格 | 
| orderState	| 1:未成交,2:部分成交,9:完全成交,19:已撤单 | 
| orderSide	| 1:买 / 2:卖 | 
| triggerPrice	| 触发价 | 
| priceGap	| 回调差价 | 
| orderMode	| 订单模式(普通:0; 止损:10; 止盈:11; 移动止损:20) | 
| orderProperty	| 订单属性:orderProperty:(普通:0; FAK:1; FOK:2; 摆盘:3) | 
| pageIndex	| 当前页码 | 
| pageSize	| 每页数据条数 | 
| total	| 总记录数 | 

### 获取24小时之前订单信息

#### 请求

`POST https://openapi.idax.pro/api/v2/beforeOrderHistory`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	 | String	|是	| IDAX支持的交易对 |
| orderState	| Integer	|是	| 查询状态。-1:所有状态, 0：未完成状态, 1：已完成状态。（只返回最近两天的数据） |
| orderSide	| Integer	|是	| 0 全部 1 买 2 卖 |
| startTime	| Long	|是	| 开始时间 |
| endTime	| Long	|是	| 结束时间 |
| pageIndex	| Integer	|否	| 当前页数 |
| pageSize	| Integer	|否	| 每页条数，最多1000条 |
| key	| String	|是	| 用户申请的key |
| sign	| String	|是	| 请求参数的签名 |
| timestamp	| Long	|是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"Successful request processing",
    "orders": [
        {
        "avgPrice": "string",
        "dealQuantity": "string",
        "orderId": 0,
        "orderMode": 0,
        "orderProperty": 0,
        "orderSide": "string",
        "orderState": 0,
        "price": "string",
        "priceGap": 0,
        "quantity": "string",
        "timestamp": 0,
        "triggerPrice": 0
        }
    ],
    "pageIndex": 1,
    "pageSize": 1,
    "total": 3
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| orders	| 订单信息 |
| quantity	| 委托数量 |
| avgPrice	| 平均成交价 |
| timestamp	| 委托时间 |
| dealQuantity	| 成交数量 |
| orderId	| 订单ID |
| price	| 委托价格 |
| orderState	| 1:未成交,2:部分成交,9:完全成交,19:已撤单 |
| orderSide	| 1:买 / 2:卖 |
| triggerPrice	| 触发价 |
| priceGap	| 回调差价 |
| orderMode	| 订单模式(普通:0; 止损:10; 止盈:11; 移动止损:20) |
| orderProperty	| 订单属性:orderProperty:(普通:0; FAK:1; FOK:2; 摆盘:3) |
| pageIndex	| 当前页码 |
| pageSize	| 每页数据条数 |
| total	| 总记录数 |

### 获取交易历史信息

#### 请求

`POST https://openapi.idax.pro/api/v2/tradesHistory`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| since	| String	| 是	| since:交易ID，从指定交易id开始获取；未指定时，返回最新成交信息（最多600条数据） |
| key	| String	| 是	| 用户申请的key |
| sign	| String	| 是	| 请求参数的签名 |
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "trades":[{
        "timestamp": 1367130137,
        "price": "787.71",
        "quantity": "0.003",
        "id": 2304331234,
        "maker":"buy"
    },
    {
        "timestamp": 1367130137,
        "price": "787.71",
        "quantity": "0.003",
        "id": 2304330000,
        "maker":"sell"
    }]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| timestamp	| 成交时间(ms) |
| price	| 交易价格 |
| quantity	| 交易数量 |
| maker	| 成交类型buy / sell |
| id	| 交易记录ID |

### 获取我的历史交易信息

#### 请求

`POST https://openapi.idax.pro/api/v2/myTrades`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| orderSide	| String	| 否	| buy:买，sell:卖 |
| startDate	| Long	| 否	| 开始时间戳 |
| endDate	| Long	| 否	| 结束时间戳 |
| pageIndex	| Integer	| 否	| 当前页数 |
| pageSize	| Integer	| 否	| 每页条数，最多1000条 |
| key	| String	| 是	| 用户申请的key |
| sign	| String	| 是	| 请求参数的签名 |
| timestamp	| Long	| 是	| 请求时间戳（3分钟内有效） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "trades":[{
        "timestamp": 1367130137,
        "price": "787.71",
        "quantity": "0.003",
        "pair": "ETH_BTC",
        "maker":"buy"
    },
    {
        "timestamp": 1367130137,
        "price": "787.71",
        "quantity": "0.003",
        "pair": "EOS_BTC",
        "maker":"sell"
    }],
    "total": 2
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| timestamp	 | 成交时间(ms) |
| price	| 交易价格 |
| quantity	| 交易数量 |
| maker	| 成交类型buy / sell |
| pair	| 交易对 |


## 公共接口

### 获取服务器时间戳

#### 请求

`GET https://openapi.idax.pro/api/v2/systemTime`




#### 请求参数

无

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "timestamp":1417449600000
} 
```

| 参数名         |         描述         |
|---------------|--------------------|
| timestamp	| 1417449600000：服务器时间戳 |

### 签名算法验证

#### 请求

`GET https://openapi.idax.pro/api/v2/getSign`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| needSignature	| String	| 是	| 待签名数据 | 

#### 返回参数

>响应

```json
{
    "code": 10000,
    "msg": "Successful request processing",
    "sign": "907ab44a485727b604354da47acc6e54e1abeff73defcaa282f38c50b21877ec"
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| sign	| 签名 |

### 获取市场行情

#### 请求

`POST https://openapi.idax.pro/api/v2/ticker`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 否	| IDAX支持的交易对。指定pair时返回特定交易对市场行情；未指定pair时返回全部交易对市场行情；|

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "timestamp":1417449600000,
    "ticker":[{
        "pair":"ETH_BTC"
        "open":"33.15",
        "high":"34.15",
        "low":"32.05",
        "last":"33.15",
        "vol":"1053.3919"
    }]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| timestamp	| 服务器时间 |
| pair	| 交易对名称 |
| open	| 开盘价 |
| high	| 最高价 |
| low	| 最低价 |
| last	| 最新成交价 |
| vol	| 成交量(最近的24小时) |

### 获取市场深度行情

#### 请求

`GET https://openapi.idax.pro/api/v2/depth`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| size	| Integer	| 否	| value: 1-200，单向条数（默认100） |
| merge	| Integer	| 否	| value: 0~8(行情页面不同交易对支持的小数位可能不同)，合并小数位（默认0.00000001深度） |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "asks": [
        ["792", "5"],
        ["789.68", "0.018"],
        ["788.99", "0.042"],
        ["788.43", "0.036"],
        ["787.27", "0.02"]
    ],
    "bids": [
        ["787.1", "0.35"],
        ["787", "12.071"],
        ["786.5", "0.014"],
        ["786.2", "0.38"],
        ["786", "3.217"]
    ]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| asks	| 卖方深度[价格，数量] |
| bids	| 买方深度[价格，数量] |

### 获取K线数据

#### 请求

`GET https://openapi.idax.pro/api/v2/kline`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| period	| Long	| 是	| 1min,5min,15min,30min,1hour,2hour,4hour,6hour,12hour,1day,1week |
| size	| Integer	| 否	| 指定获取数据的条数(默认全部获取) |
| since	| Long	| 否	| 时间戳。返回指定时间戳前的数据(默认全部获取) |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "kline":[
        1417449600000,
        "2339.11",
        "2383.15",
        "2322",
        "2369.85",
        "83850.06"
    ]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| 1417449600000	| 时间戳 |
| 2370.16	| 开 |
| 2380	| 高 |
| 2352	| 低 |
| 2367.37	| 收 |
| 17259.83	| 交易量 |

### 获取最近成交信息

#### 请求

`GET https://openapi.idax.pro/api/v2/trades`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 是	| IDAX支持的交易对 |
| total	| int	| 否	| 指定获取的总条数 |

#### 返回参数

>响应

```json
{
    "code":10000,
    "msg":"request success",
    "trades":[{
        "timestamp": 1417449600000,
        "price": "787.71",
        "quantity": "0.003",
        "id": "230433",
        "maker": "buy"
    },
    {
        "timestamp": 1417449600000,
        "price": "787.71",
        "quantity": "0.003",
        "id": "230433",
        "maker": "sell"
    }]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| timestamp	| 成交时间(ms) |
| price	| 交易价格 |
| quantity	| 交易数量 |
| id	| 交易记录ID |
| maker	| 成交类型buy / sell |

### 获取交易对列表

#### 请求

`GET https://openapi.idax.pro/api/v2/pairs`




#### 请求参数

无

#### 返回参数

>响应

```json
{
    "code": 10000,
    "msg": "Successful request processing",
    "pairs": ["DROP_BTC", "VIBE_ETH", "PAT_ETH", "ELEC_ETH", "TRX_USDT", "ANON_ETH", "LTC_BTC", "NER_ETH", "FML_ETH",
        "C2C_BTC", "MTV_USDT", "AE_ETH", "EOS_BTC", "REN_ETH", "RET_USDT", "LRN_BTC", "MRPH_BTC", "VRS_ETH", "LINDA_BTC",
        "WT_ETH", "ZAT_BTC", "SDL_ETH", "FTL_ETH", "OCT_BTC", "VEX_BTC", "TPP_BTC", "WIRE_BTC", "SPD_BTC", "GBC_BTC",
        "STORJ_BTC", "BRN_ETH", "WIRE_USDT", "HERC_ETH", "WT_BTC", "ABBC_BTC", "EVE_ETH", "TPP_ETH", "CLO_ETH", "VIBE_USDT",
        "EOS_ETH", "XRP_USDT", "AEN_ETH", "STSC_USDT", "DROP_ETH", "WAB_BTC", "ZAT_ETH", "OMG_ETH", "UQC_ETH", "WIT_ETH",
        "RET_ETH", "OCT_USDT", "PX_USDT", "LRN_ETH", "MTV_BTC", "VRS_BTC", "CLO_BTC", "CTC_BTC", "OST_BTC", "SNT_ETH",
        "TIC_BTC", "NRC_BTC", "TRX_ETH", "BAAS_USDT", "HERC_USDT", "RET_BTC", "TRF_ETH", "EDR_BTC", "DLP_BTC", "ZEN_BTC",
        "VIN_ETH", "BOC_ETH", "XET_BTC", "ZING_BTC", "ETC_USDT", "COS_ETH", "GUPASS_ETH", "AE_BTC", "ABTC_BTC", "NULS_BTC",
        "TRF_BTC", "VIN_BTC", "ARP_ETH", "UCN_USDT", "LRC_ETH", "FEX_USDT", "OMG_BTC", "UQC_BTC", "HYDRO_ETH", "WIRE_ETH",
        "TKGN_ETH", "ABTC_ETH", "BCHABC_BTC", "MTV_ETH", "AEN_BTC", "MRPH_ETH", "LIGHT_ETH", "DASC_BTC", "EVC_ETH", "ETH_USDT",
        "STORJ_ETH", "TIC_ETH", "PCCM_BTC", "SPD_ETH", "NRC_ETH", "AIC_USDT", "HBZ_ETH", "TBL_ETH", "EVE_BTC", "BCHSV_BTC",
        "FML_BTC", "DIY_ETH", "XET_ETH", "XRP_ETH", "NER_BTC", "KRM_USDT", "MO_BTC", "PLAN_BTC", "DEX_BTC", "DASC_ETH", "FEX_ETH",
        "MGO_BTC", "ONTOP_ETH", "ZING_ETH", "POE_BTC", "DIY_USDT", "CYFM_ETH", "PPI_ETH", "SFU_BTC", "CAPP_BTC", "FIC_BTC",
        "NULS_ETH", "CPLO_BTC", "TAT_BTC", "EAE_USDT", "NULS_USDT", "VIT_BTC", "FTX_ETH", "LRC_BTC", "ZEN_USDT", "HYDRO_BTC",
        "STSC_BTC", "TMC_ETH", "PHR_ETH", "NFFC_ETH", "TRX_BTC", "NTY_USDT", "SCRL_BTC", "CAN_BTC", "PCCM_ETH", "VIT_ETH",
        "EVC_BTC", "HBZ_BTC", "ZEN_ETH", "ARN_ETH", "PHR_BTC", "AIC_ETH", "SNT_BTC", "DLP_USDT", "SCRL_ETH", "CTC_ETH",
        "DEX_ETH", "MEDIBIT_ETH", "FOTA_BTC", "DPN_ETH", "PSM_BTC", "TUSD_BTC", "NPX_BTC", "DLP_ETH", "FUNDZ_BTC", "FTX_BTC",
        "PIX_BTC", "PX_BTC", "FOTA_ETH", "UCN_ETH", "ILC_ETH", "GNAIT_USDT", "ADK_BTC", "PIX_ETH", "XHV_BTC", "WICC_ETH",
        "APT_BTC", "TMC_USDT", "NPX_ETH", "IOST_BTC", "PX_ETH", "VIBE_BTC", "HERC_BTC", "SW_BTC", "NIX_BTC", "MCT_ETH",
        "APL_ETH", "LIGHT_USDT", "WICC_USDT", "EVE_USDT", "TUSD_USDT", "SHIFT_ETH", "NIX_USDT", "CAPP_USDT", "TAT_ETH",
        "WAB_ETH", "SW_ETH", "BAAS_BTC", "ADK_ETH", "TMC_BTC", "MEDIBIT_BTC", "OCT_ETH", "DPN_BTC", "HQT_BTC", "REN_BTC",
        "PSM_ETH", "PPI_BTC", "STSC_ETH", "FID_BTC", "C2C_USDT", "FUNDZ_ETH", "ANON_BTC", "BTC_USDT", "TUSD_ETH", "LTC_USDT",
        "WIT_BTC", "LGC_ETH", "APL_BTC", "DROP_USDT", "SHIFT_BTC", "ILC_BTC", "CAN_ETH", "XHV_ETH", "UCN_BTC", "GBC_ETH",
        "PAT_BTC", "CAPP_ETH", "APIS_BTC", "ETH_BTC", "ARN_BTC", "FIC_ETH", "APT_ETH"]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| pairs	| "DROP_BTC"：交易对 | 

### 获取交易对配置

#### 请求

`GET https://openapi.idax.pro/api/v2/pairLimits`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 否	| IDAX支持的交易对 |

#### 返回参数

>响应

```json
{
    "code": 10000,
    "msg": "request success",
    "pairRuleVo": [{
        "pairName": "ETH_BTC",
        "maxAmount": "1000000000000.00000000",
        "minAmount": "0.00100000",
        "priceDecimalPlace": 6,
        "qtyDecimalPlace": 3
    }]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| pairName	| 交易对 |
| maxAmount	| 最大金额 |
| minAmount	| 最小金额 |
| priceDecimalPlace	| 价格小数位 |
| qtyDecimalPlace	| 数量小数位 |

### 获取报价币估值

#### 请求

`GET https://openapi.idax.pro/api/v2/pairRate`




#### 请求参数

无

#### 返回参数

>响应

```json
{
    "code": 10000,
    "msg": "Successful request processing",
    "marketValuationCnies": [{
        "coinName": "BTC",
        "cny": "35203.948166",
        "usd": "5238.89637256"
    }, {
        "coinName": "ETH",
        "cny": "1209.3710803",
        "usd": "179.973272765"
    }, {
        "coinName": "USDT",
        "cny": "6.8006186881",
        "usd": "1.0120380932"
    }]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| coinName	| 货币名称 |
| cny	| 人民币 |
| usd	| usd |

### 获取支持的Coin/Token列表

#### 请求

`GET https://openapi.idax.pro/api/v2/coinList`




#### 请求参数

无

#### 返回参数

>响应

```json
{
	"code": 10000,
	"msg": "Successful request processing",
	"coinList": [{
		"coinCode": "BTC",
		"coinName": "Bitcoin",
		"canDeposit": true,
		"canWithdraw": true,
		"minWithdrawal": 0.00100000
	}, {
		"coinCode": "ETH",
		"coinName": "Ethereum",
		"canDeposit": true,
		"canWithdraw": true,
		"minWithdrawal": 0.01000000
	}, {
		"coinCode": "EOS",
		"coinName": "EOS",
		"canDeposit": true,
		"canWithdraw": true,
		"minWithdrawal": 1.00000000
	}]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| coinCode	| 币种代码 |
| coinName	| 币种名称 |
| canDeposit	| 是否允许充值 |
| canWithdraw	| 是否允许提现 |
| minWithdrawal	| 最小提现额度 |

### 获取支持的交易对信息

#### 请求

`GET curl https://openapi.idax.pro/api/v2/pairList?pair=ETH_BTC`




#### 请求参数

| 参数名        | 参数类型 | 必填 |         描述         |
|---------------|----------|----------|--------------------|
| pair	| String	| 否	| IDAX支持的交易对 |

#### 返回参数

>响应

```json
{
	"code": 10000,
	"msg": "Successful request processing",
	"pairList": [{
		"pairName": "ETH_BTC",
		"status": "Open",
		"maxAmount": "1000000000000.00000000",
		"minAmount": "0.00100000",
		"priceDecimalPlace": 6,
		"qtyDecimalPlace": 3,
		"baseCoinCode": "ETH",
		"quoteCoinCode": "BTC"
	}]
}
```

| 参数名         |         描述         |
|---------------|--------------------|
| pairName	| 交易对 |
| status	| 状态 |
| maxAmount	| 最大金额 |
| minAmount	| 最小金额 |
| priceDecimalPlace	| 价格小数位 |
| qtyDecimalPlace	| 数量小数位 |
| baseCoinCode	| 基准币Code |
| quoteCoinCode	| 报价币Code |