# Token Trading REST API 

## Private Interface

### Get Account Information

#### Request

`POST https://openapi.idax.pro/api/v2/userinfo`


#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|:--------------------:|
| key	| String	| Yes	| API Key |
| sign	| String	| Yes	| Signature of the request parameter |
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|:--------------------:|
| total	| Total assets (ms) |
| free	| Available |
| freezed	| Frozen |

### Placing an order

#### Request

`POST https://openapi.idax.pro/api/v2/placeOrder`

<aside class="notice">

Place an order to trade (except for this API call, all other API calls use quantity to represent “quantity” and amount to represent “amount”.)

</aside>


#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	 | Yes	  | Trading pairs supported by IDAX |
| orderType	| String	 | Yes	  | limit: limit price, market: market price |
| orderSide	| String	 | Yes	  | buy: bid, sell: ask |
| price	| Double	 | Yes	  | Order price |
| amount	| Double	 | Yes	  | Order quantity |
| triggerPrice	| Double	 | No	  | TriggerPrice(trigger Price must be greater than 0) |
| priceGap	| Double	 | No	  | Price Gap (if It's trailing stop order, the price gap must be greater than 0).(Limited stop-loss, stop earnings do not support priceGap) |
| orderMode	| Integer	 | Yes	  | Order mode: standard mode:0; stop loss:10; take profit:11; trailing stop:20 |
| orderProperty	| Integer	 | Yes	  | OrderProperty : Standard property:0; FAK:1; FOK:2; POST-ONLY:3 |
| key	| String	 | Yes	  | API Key |
| sign	| String	 | Yes	  | Signature of the request parameter |
| timestamp	| Long	 | Yes	  | Request timestamp (effective within 3 minutes) |

#### Response

>Response

```json
{
    "code":10000,
    "msg":"request success", 
    "orderId":"2000000000008832432"
}
```

| Parameters         |         Description         |
|---------------|:--------------------:|
| orderId	| Order Id | 

### Cancel an order

#### Request

`POST https://openapi.idax.pro/api/v2/cancelOrder`

<aside class="notice">

Order cancellation (batch cancellation supported)

</aside>


#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| orderId	| String	| Yes |	Order ID (multiple order IDs are separated by "," and it is allowed to cancel up to five orders at a time) |
| key	| String	| Yes |	API Key |
| sign	| String	| Yes |	Signature of the request parameter |
| timestamp	| Long	| Yes |	Request timestamp (effective within 3 minutes) |

#### Response

>Response

```json
{
    "code":10000,
    "msg":"request success",
    "accepted":"123456789"
}
```

| Parameters         |         Description         |
|---------------|:--------------------:|
| accepted	| Order number (the last number of the orders that have accepted the cancellation request) |

### Get order information

#### Request

`POST https://openapi.idax.pro/api/v2/orderInfo`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| orderId	| String	| Yes	| Order ID. -1: All unfinished orders; otherwise query the specified orders. |
| pageIndex	| int	| No	| Current page number |
| pageSize	| int	| No	| The number of strings on each page. |
| key	| String	| Yes	| API Key |
| sign	| String	| Yes	| Signature of the request parameter |
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| orders	| Order information |
| quantity	| Order placement quantity |
| avgPrice	| Average deal price |
| timestamp	| Order placement time |
| dealQuantity	| Deal quantity |
| orderId	| Order ID |
| price	| Order placement price |
| orderState	| 1: Not filled, 2: partially filled, 9: totally filled, 19: cancelled |
| orderSide	| 1:buy / 2:sell |
| triggerPrice	| Trigger Price |
| priceGap	| Price Gap |
| orderMode	| Order mode: standard mode:0; stop loss:10; take profit:11; trailing stop:20 |
| orderProperty	| OrderProperty : Standard property:0; FAK:1; FOK:2; POST-ONLY:3 |
| total	| Total records |

### Get Order List

#### Request

`POST https://openapi.idax.pro/api/v2/orderList`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| orderId	| String	| Yes	| Order ID (Multiple order IDs are separated by) |
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| pageIndex	| int	| No	| Current page number |
| pageSize	| int	| No	| The number of strings on each page. |
| key	| String	| Yes	| API Key |
| sign	| String	| Yes	| Signature of the request parameter |
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| orders	| Order information |
| quantity	| Order placement quantity |
| avgPrice	| Average deal price |
| timestamp	| Order placement time |
| dealQuantity	| Deal quantity |
| orderId	| Order ID |
| price	| Order placement price |
| orderState	| 1: Not filled, 2: partially filled, 9: totally filled, 19: cancelled |
| orderSide	| 1:buy / 2:sell |
| triggerPrice	| Trigger Price |
| priceGap	| Price Gap |
| orderMode	| Order mode: standard mode:0; stop loss:10; take profit:11; trailing stop:20 |
| orderProperty	| OrderProperty : Standard property:0; FAK:1; FOK:2; POST-ONLY:3 |

### Get Order Information in the Latest 24 Hours

#### Request

`POST https://openapi.idax.pro/api/v2/orderHistory`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| orderState	| Integer	| Yes	| Query status.
-1: All statuses,
0: unfinished state,
1: completed state. | 
| pair	| String	| Yes	| Trading pairs supported by IDAX | 
| pageIndex	| Integer	| No	| Current page number | 
| pageSize	| Integer	| No	| The maximum number of strings on each page is 1,000. | 
| key	| String	| Yes	| API Key | 
| sign	| String	| Yes	| Signature of the request parameter | 
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) | 

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| orders	| Order information | 
| quantity	| Order placement quantity | 
| avgPrice	| Average deal price | 
| timestamp	| Order placement time | 
| dealQuantity	| Deal quantity | 
| orderId	| Order ID | 
| price	| Order placement price | 
| orderState	| 1: Not filled, 2: partially filled, 9: totally filled, 19: cancelled | 
| orderSide	| 1:buy / 2:sell | 
| triggerPrice	| Trigger Price | 
| priceGap	| Price Gap | 
| orderMode	| Order mode: standard mode:0; stop loss:10; take profit:11; trailing stop:20 | 
| orderProperty	| OrderProperty : Standard property:0; FAK:1; FOK:2; POST-ONLY:3 | 
| pageIndex	| Current page number | 
| pageSize	| 	Page length | 
| total	| Total records | 

### Get Order Information before the Latest 24 Hours

#### Request

`POST https://openapi.idax.pro/api/v2/beforeOrderHistory`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	 | String	|Yes	| Trading pairs supported by IDAX |
| orderState	| Integer	|Yes	| Query status.
-1: All statuses,
0: unfinished state,
1: completed state. |
| orderSide	| Integer	|Yes	| 0 All 1 buy 2 sell |
| startTime	| Long	|Yes	| start time |
| endTime	| Long	|Yes	| End time |
| pageIndex	| Integer	|No	| Current page number |
| pageSize	| Integer	|No	| The maximum number of strings on each page is 1,000. |
| key	| String	|Yes	| API Key |
| sign	| String	|Yes	| Signature of the request parameter |
| timestamp	| Long	|Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| orders	| Order information |
| quantity	| Order placement quantity |
| avgPrice	| Average deal price |
| timestamp	| Order placement time |
| dealQuantity	| Deal quantity |
| orderId	| Order ID |
| price	| Order placement price |
| orderState	| 1: Not filled, 2: partially filled, 9: totally filled, 19: cancelled |
| orderSide	| 1:buy / 2:sell |
| triggerPrice	| Trigger Price |
| priceGap	| Price Gap |
| orderMode	| Order mode: standard mode:0; stop loss:10; take profit:11; trailing stop:20 |
| orderProperty	| OrderProperty : Standard property:0; FAK:1; FOK:2; POST-ONLY:3 |
| pageIndex	| Current page number |
| pageSize	| Page length |
| total	| Total records |

### Get historical transaction information

#### Request

`POST https://openapi.idax.pro/api/v2/tradesHistory`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| since	| String	| Yes	| since: The transaction ID is obtained from the specified transaction ID; when no ID is specified, the latest transaction information (up to 600 strings of data) is returned. |
| key	| String	| Yes	| API Key |
| sign	| String	| Yes	| Signature of the request parameter |
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| timestamp	| Deal time(ms) |
| price	| Transaction price |
| quantity	| Transaction quantity |
| maker	| Deal type: buy/sell |
| id	| Transaction record ID |

### Get my historical transaction information

#### Request

`POST https://openapi.idax.pro/api/v2/myTrades`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| orderSide	| String	| No	| buy: bid, sell: ask |
| startDate	| Long	| No	| start time |
| endDate	| Long	| No	| End time |
| pageIndex	| Integer	| No	| Current page number |
| pageSize	| Integer	| No	| The maximum number of strings on each page is 1,000. |
| key	| String	| Yes	| API Key |
| sign	| String	| Yes	| Signature of the request parameter |
| timestamp	| Long	| Yes	| Request timestamp (effective within 3 minutes) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| timestamp	 | Deal time(ms) |
| price	| Transaction price |
| quantity	| Transaction quantity |
| maker	| Deal type:buy / sell |
| pair	| Trading Pair |


## Public Interface

### Get server timestamp

#### Request

`GET https://openapi.idax.pro/api/v2/systemTime`




#### Request

None

#### Response

>Response

```json
{
    "code":10000,
    "msg":"request success",
    "timestamp":1417449600000
} 
```

| Parameters         |         Description         |
|---------------|--------------------|
| timestamp	| 1417449600000：Server timestamp |

### Signature算法验证

#### Request

`GET https://openapi.idax.pro/api/v2/getSign`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| needSignature	| String	| Yes	| Data to be signed | 

#### Response

>Response

```json
{
    "code": 10000,
    "msg": "Successful request processing",
    "sign": "907ab44a485727b604354da47acc6e54e1abeff73defcaa282f38c50b21877ec"
}
```

| Parameters         |         Description         |
|---------------|--------------------|
| sign	| Signature |

### Get market tickers

#### Request

`GET https://openapi.idax.pro/api/v2/ticker`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| No	| Trading pairs supported by IDAX. Market tickers of specific transaction pairs are returned when pairs are specified; Market tickers of all transaction pairs are returned when no pairs are specified; |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| timestamp	| Server time |
| pair	| Trading Pair |
| open	| Opening price |
| high	| Highest price |
| low	| Lowest price |
| last	| Latest deal price |
| vol	| Volume (within the latest 24 hours) |

### Get depth tickers

#### Request

`GET https://openapi.idax.pro/api/v2/depth`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| size	| Integer	| No	| value: 1-200，Unidirectional strings(100 by default) |
| merge	| Integer	| No	| value: 0~8 (the decimal place supported by different trading on the ticker pairs may be different), decimal places merged(by default, 8) |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| asks	| Seller depth [price, quantity] |
| bids	| Buyer depth [price, quantity] |

### Get K-line data

#### Request

`GET https://openapi.idax.pro/api/v2/kline`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| period	| Long	| Yes	| 1min,5min,15min,30min,1hour,2hour,4hour,6hour,12hour,1day,1week |
| size	| Integer	| No	| Specify the number of strings of data to be obtained(all are obtained by default) |
| since	| Long	| No	| Timestamp |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| 1417449600000	| timestamp |
| 2370.16	| Opening |
| 2380	| Highest |
| 2352	| Lowest |
| 2367.37	| Closing |
| 17259.83	| Trade volume |

### Get the latest deal information

#### Request

`GET https://openapi.idax.pro/api/v2/trades`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| Yes	| Trading pairs supported by IDAX |
| total	| int	| No	| Total records |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| timestamp	| Deal time(ms) |
| price	| Transaction price |
| quantity	| Transaction quantity |
| id	| Transaction record ID |
| maker	| Deal type:buy / sell |

### Get list of trading pairs

#### Request

`GET https://openapi.idax.pro/api/v2/pairs`




#### Request

None

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| pairs	| "DROP_BTC"：Trading Pair | 

### Get trading pair configuration

#### Request

`GET https://openapi.idax.pro/api/v2/pairLimits`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| No	| Trading pairs supported by IDAX |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| pairName	| Trading Pair |
| maxAmount	| Maximum amount |
| minAmount	| Minimum amount |
| priceDecimalPlace	| Price decimal place |
| qtyDecimalPlace	| Quantity decimal place |

### Get quote currency valuation

#### Request

`GET https://openapi.idax.pro/api/v2/pairRate`




#### Request

None

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| coinName	| Currency Name |
| cny	| CNY |
| usd	| usd |

### Get the Coin/Token list

#### Request

`GET https://openapi.idax.pro/api/v2/coinList`




#### Request

None

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| coinCode	| coinCode |
| coinName	| coinName |
| canDeposit	| canDeposit |
| canWithdraw	| canWithdraw |
| minWithdrawal	| minWithdrawal |

### Get support exchange pair list

#### Request

`GET curl https://openapi.idax.pro/api/v2/pairList?pair=ETH_BTC`




#### Request

| Parameters        | Type | Required |         Description         |
|---------------|----------|----------|--------------------|
| pair	| String	| No	| Trading pairs supported by IDAX |

#### Response

>Response

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

| Parameters         |         Description         |
|---------------|--------------------|
| pairName	| Trading Pair |
| status	| pair status |
| maxAmount	| Maximum amount |
| minAmount	| Minimum amount |
| priceDecimalPlace	| Price decimal place |
| qtyDecimalPlace	| Quantity decimal place |
| baseCoinCode	| Base Coin Code |
| quoteCoinCode	| Quote Coin Code |