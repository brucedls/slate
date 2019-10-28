# WebSocket API

## Subscribe to trend data

#### Request

>Request


```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_ticker'}");

```

<aside class="notice">
X value: Transaction pairs supported by IDAX (Monitoring is case-insensitive, but the message is lowercase when pushed.)
</aside>

#### Request Parameter

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| event | String   | Yes       | Subscribe event |
| channel	| String	| Yes	| Subscribe channel |

#### Response

>Response

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

| Parameters        | Type |         Description         |
|---------------|----------|:--------------------:|
| timestamp	| long | Server time  |
| open	| double | Opening price |
| high	| double | Highest price |
| low	| double | Lowest price |
| last	| double | Latest deal price |
| vol	| double | Volume (within the latest 24 hours) |

## Subscribe to market depth (return incremental data)

#### Request

>Request

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_depth'}");
```

<aside class="notice">
X value: Transaction pairs supported by IDAX (Monitoring is case-insensitive, but the message is lowercase when pushed.)
</aside>

#### Request Parameter

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| event | String   | Yes       | Subscribe event |
| channel	| String	| Yes	| Subscribe channel |

#### Response

>Response

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

| Parameters        | Type |         Description         |
|---------------|----------|:--------------------:|
| bids | [string, string]   | Buyer depth |
| asks	| [string, string] | Seller depth  |
| timestamp	| long | Server timestamp |

Use description: Firstly, the whole market depth data are returned (<=200 price levels), then the incremental data will be returned based on the server data in following three ways:

* Delete (when the quantity is 0.),
* Amend (When the prices are the same while the quantities are different.),
* Increase (When new price presents.)

## Subscribe to market depth (pushed once per second)

#### Request

>Request

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_depth_Y'}");
```

<aside class="notice">
X value: Transaction pairs supported by IDAX (Monitoring is case-insensitive, but the message is lowercase when pushed.)
<br>
Y value: 5, 10, 20, 50(Market depth)
</aside>

#### Request Parameter

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| event | String   | Yes       | Subscribe event |
| channel	| String	| Yes	| Subscribe channel |

#### Response

>Response

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

| Parameters        | Type |         Description         |
|---------------|----------|:--------------------:|
| bids | [string, string]   | Buyer depth |
| asks	| [string, string] | Seller depth  |
| timestamp	| long | Server timestamp |

## Subscribe to deal records

#### Request

>Request

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_trades'}");
```

<aside class="notice">
X value: Transaction pairs supported by IDAX (Monitoring is case-insensitive, but the message is lowercase when pushed.)
</aside>

#### Request Parameter

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| event | String   | Yes       | Subscribe event |
| channel	| String	| Yes	| Subscribe channel |

#### Response

>Response

```json
    {
    "channel":"idax_sub_eth_btc_trades",
    "data":[["1001","2463.86","0.052",1411718972024,"buy"]]
    }
```

| Parameters        | Type |         Description         |
|---------------|----------|:--------------------:|
| channel	| String	| Subscribe channel |
| data	| [String, String] | Transaction serial number, price, volume, deal time, transaction type (buy | sell)  |

## Subscribe to K-line data

#### Request

>Request

```
websocket.send("{
 'event':'addChannel',
 'channel':'idax_sub_X_kline_Y'}");
```

<aside class="notice">

X value: Transaction pairs supported by IDAX (Monitoring is case-insensitive, but the message is lowercase when pushed.)
Y值为: 1min, 5min, 15min, 30min, 1hour, 2hour, 4hour, 6hour, 12hour, 1day, 1week

</aside>

#### Request Parameter

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| event | String   | Yes       | Subscribe event |
| channel	| String	| Yes	| Subscribe channel |

#### Response

>Response

```json
    {
    "channel":"idax_sub_eth_btc_kline_1min",
    "data":[
        ["1490337840000","995.37","996.75","995.36","996.75","9.112"],
        ["1490337840000","995.37","996.75","995.36","996.75","9.112"]
    ]
    }
```

| Parameters        | Type |         Description         |
|---------------|----------|:--------------------:|
| channel	| String	| Subscribe channel |
| data	| [String, String] | Timestamp, opening price, highest price, lowest price, closing price, deal volume  |


