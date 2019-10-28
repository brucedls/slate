# WebSocket API

## 订阅行情数据

#### 请求

>请求


```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_ticker'}");

```

<aside class="notice">
X值为：IDAX支持的交易对(监听不区分大小写，但消息推送时为小写)
</aside>

#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| event | String   | 是       | 订阅事件 |
| channel	| String	| 是	| 订阅管道 |

#### 返回参数

>响应

```json
{
"channel":"idax_sub_eth_btc_ticker",
"data":[{
  "open":"2466",
  "high":"2555",
  "low":"2466",
  "last":"2478.51",
  "timestamp":1411718074965,
  "vol":"49020.30"
}]
}
```

| 参数名        | 参数类型 |         描述         |
|---------------|----------|:--------------------:|
| instrument_id | String   | 合约名称，如 BTC_USD |
| timestamp	| long | 服务器时间  |
| open	| double | 开盘价 |
| high	| double | 最高价 |
| low	| double | 最低价 |
| last	| double | 最新成交价 |
| vol	| double | 成交量(最近的24小时) |

## 订阅市场深度(增量数据返回)

#### 请求

>请求

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_depth'}");
```

<aside class="notice">
X值为：IDAX支持的交易对(监听不区分大小写，但消息推送时为小写)
</aside>

#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| event | String   | 是       | 订阅事件 |
| channel	| String	| 是	| 订阅管道 |

#### 返回参数

>响应

```json
{
"channel":"idax_sub_eth_btc_depth",
"data":[{
    "bids":[
        ["2473.88","2.025"],
        ["2473.5","2.4"],
        ["2470","12.203"]
    ],
    "asks":[
        ["2484","17.234"],
        ["2483.01","6"],
        ["2482.88","3"]
    ],
    "timestamp":1411718972024
}]
}
```

| 参数名        | 参数类型 |         描述         |
|---------------|----------|:--------------------:|
| bids | [string, string]   | 买方深度 |
| asks	| [string, string] | 卖方深度  |
| timestamp	| long | 服务器时间戳 |

使用描述： 第一次返回市场深度全量数据(<=200)，此后，根据服务端数据对第一次返回数据进行如下三种操作

* 删除（当订单的量变为0时），
* 修改（当订单价格相同量发生变化），
* 增加（出现新的订单价格）

## 订阅市场深度(每秒推送1次)

#### 请求

>请求

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_depth_Y'}");
```

<aside class="notice">
X值为：IDAX支持的交易对(监听不区分大小写，但消息推送时为小写)
Y值为: 5, 10, 20, 50(获取深度条数)
</aside>

#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| event | String   | 是       | 订阅事件 |
| channel	| String	| 是	| 订阅管道 |

#### 返回参数

>响应

```json
{
"channel":"idax_sub_eth_btc_depth_20",
"data":[{
    "bids":[
        ["2473.88","2.025"],
        ["2473.5","2.4"],
        ["2470","12.203"]
    ],
    "asks":[
        ["2484","17.234"],
        ["2483.01","6"],
        ["2482.88","3"]
    ],
    "timestamp":1411718972024
}]
}
```

| 参数名        | 参数类型 |         描述         |
|---------------|----------|:--------------------:|
| bids | [string, string]   | 买方深度 |
| asks	| [string, string] | 卖方深度  |
| timestamp	| long | 服务器时间戳 |

## 订阅成交记录

#### 请求

>请求

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_trades'}");
```

<aside class="notice">
X值为：IDAX支持的交易对(监听不区分大小写，但消息推送时为小写)
</aside>

#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| event | String   | 是       | 订阅事件 |
| channel	| String	| 是	| 订阅管道 |

#### 返回参数

>响应

```json
{
"channel":"idax_sub_eth_btc_trades",
"data":[["1001","2463.86","0.052",1411718972024,"buy"]]
}
```

| 参数名        | 参数类型 |         描述         |
|---------------|----------|:--------------------:|
| channel	| String	| 订阅管道 |
| data	| [String, String] | 交易序号, 价格, 成交量, 成交时间, 成交类型（buy/sell）  |

## 订阅K线数据

#### 请求

>请求

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_kline_Y'}");
```

<aside class="notice">

X值为：IDAX支持的交易对(监听不区分大小写，但消息推送时为小写)
Y值为: 1min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, 1day, 1week

</aside>

#### 请求参数

| 参数名        | 参数类型 | 是否必须 |         描述         |
|---------------|----------|----------|:--------------------:|
| event | String   | 是       | 订阅事件 |
| channel	| String	| 是	| 订阅管道 |

#### 返回参数

>响应

```json
{
"channel":"idax_sub_eth_btc_kline_1min",
"data":[
    ["1490337840000","995.37","996.75","995.36","996.75","9.112"],
    ["1490337840000","995.37","996.75","995.36","996.75","9.112"]
]
}
```

| 参数名        | 参数类型 |         描述         |
|---------------|----------|:--------------------:|
| channel	| String	| 订阅管道 |
| data	| [String, String] | 时间戳, 开盘价, 最高价, 最低价, 收盘价, 成交量  |


