# API Overview

It provides users with a set of simple and powerful development tools, designed to help users quickly and efficiently integrate IDAX tickers and transaction functions into their own applications. IDAX interface is the basis for providing services. After creating accounts on the IDAX website, developers can establish APIs with different authority based on their own needs, and use APIs to conduct automatic transactions. The following functions can be realized quickly through APIs: Once obtaining access to the interface, you can help with the development by reading the interface documentation. If you have any problems in the process of using, please contact us and we will give you a feedback as soon as possible.

## Matching engine

### Trading pair

Expressions: `XXX_YYY`，where `XXX` is the base currency while `YYY` is the quote currency.

For example: In the `ETH_BTC` pair, `ETH` is the base currency while `BTC` is the quote currency.

### Order side

* Buy
* Sell

### Order type（orderType : 限价:limit; 市价:market）

* A limit order refers to an order where the buy and sell prices are limited, that is, the maximum price is set when buying the base currency and the minimum price is set when selling the base currency. In addition to the price limit, a limit order transaction must also limit the quantity of the transaction base currency.

* A market order refers to an order where the buy or sell prices are not limited, that is, to buy or sell at the current market price, wherein the buy market order must limit the amount of the quote currency to buy; sell market order must limit the amount of the base currency to sell.

### Order mode（OrderMode : 标准模式:0; 止损:10; 止盈:11; 移动止损:20）

* Standard：Default order mode.

* A stop-loss limit order is an order that sells or closes positions at a set price after the market has reached a specified price which is used to control losses.

* Similar to a stop-loss limit order, a take-profit limit order is an order that sells or closes positions at a set price after the market has reached a specified price which is used to control profits.

* A stop-loss market order is an order that sells or closes positions at the market price after the market has reached a specified price which is used to control losses.

* Similar to a stop-loss market order, a take-profit market order is an order that sells or closes positions at the market price after the market has reached a specified price which is used to control profits.

* A trailing stop order is similar to stop market order, where you need to specify the trailing value differential to the trigger price, and if triggered then a market order will be placed.

Note: a positive Trail value indicates a trailing buy whilst a negative trail value indicates a trailing sell.

### Order property（OrderProperty : 标准属性:0; FAK:1; FOK:2; POST-ONLY:3）：

* Standard : Default order property

* FAK (Fill and Kill) - FAK orders are immediately executed against resting orders. Any quantity that remains unfilled is canceled.

Please note that only limit order, stop-loss limit order and take-profit limit order are supported.

* FOK (Fill or Kill) - FOK order is an order to buy or sell an instrument that must be executed immediately in its entirety; otherwise, the entire order is canceled; no partial fulfillments are allowed.

Please note that only limit order, stop-loss limit order and take-profit limit order are supported.

* POST-ONLY: The post-only limit order option ensures the limit order will be added to the order book and not match with a pre-existing order. If your order would cause a match with a pre-existing order, your post-only limit order will be canceled.

Please note that only limit order, stop-loss limit order and take-profit limit order are supported.


### Rules

* Matching rule: The matching rule of the IDAX matching engine: Priority for orders based on price and time. In specific, to match according to the priority of price over time, and to match according to the order placement time when the prices are the same, and the orders placed first will be matched first.

* Execution Price rule: When the orders are matched, the transaction prices will be based on the maker price instead of the taker price.

### Order life cycle

1. When an order first presents on the matching engine, it is in the status of "unfilled".

2. If an order is filled partially, it will come into the canceled of "partially filled", and the remaining order will stay in the queue waiting to be matched.

3. If an order is completely filled, it will come into the status of "filled".

4. If an order, in the queue waiting to be matched, is canceled, it will come into the status of "canceled" and its previous order status may be "partially filled".

Orders that are canceled or completely filled will be removed from the matching queue.

## Fee

Fee Schedule：`https://www.idax.pro/fee`

 
## Server

In order to minimize API access latency, it is recommended that you use the server in Hong Kong.

 
## Request

### REST API 

Root URL of REST access `http://openapi.idax.pro/api/v2`

All requests are based on the HTTPS protocols, and the content type in the request header must be uniformly set as: `'Application/JSON'`.

Note to request interaction (Return values of REST API service all contain “code” and “msg”)：

1. Request parameter encapsulation: Parameters are encapsulated based on the requirements of interface request parameters.

2. Request parameters submission: The encapsulated request parameters are submitted to the server by POST or GET methods.

3. Server response: The server will validate the request and return the response data in JSON format to the user according to the business logic.

4. Response data processing: Users may process the server response data on demand.


### WebSocket API

#### Link address 

`wss://openws.idax.pro/ws`

 
#### Request data format

```
{
    'event':'addChannel',
    'channel':'channelValue',
    'parameters':
    {
        'apiKey':'value1',
        'timestamp':'value2',
        'sign':'value3'
    }
}
```
* event: addChannel (register request data)/removeChannel (cancel request data)/clearChannel(cancel all requests)
* Channel: IDAX provides the request data type.
* Parameters are optional, reserved for transaction services, wherein the ‘apiKey’ is the apiKey applied for by the user. The ‘sign’ is the signature character strings, and please refer to the request note for the signature rules.
 
#### Single registration

`websocket.send("{'event':'addChannel','channel':'idax_sub_eth_btc_ticker'}")`

 
#### Batch registration

`websocket.send("[ {'event':'addChannel','channel':'idax_sub_eth_btc_ticker'}, {'event':'addChannel','channel':'idax_sub_eth_btc_depth'}, {'event':'addChannel','channel':'idax_sub_eth_btc_trades'} ]");`

 
#### Server response

The format of the return data is:

`[{"channel":"channel","code":"","data":[{}]}, {"channel":"channel","code":"","data":[{}]}]`

* channel: request data type
* code: "00000": Registration/cancellation succeeds. Other registration/cancellation fails.
* Data: return result data


 
#### Note to the WebSocket subscriptions process

* In order to ensure the timeliness of the push process and to reduce data amount, we just push ticker data (market data）and order book depth data（marekt depth）whenever the market data changes, and incremental Order Execution data（trades）. Transaction records（trades）are the incremental data generated by the last push to the current one.
* When each subscription is successful, the current data will be pushed once and the ‘code’ will indicate whether the subscription was successful or not.
* When each unsubscription is successful, the server also pushes a message with the code of success.
* IDAX will push the full order book data per second with idax_sub_X_depth_Y.

 
## Pagination

Some APIs provide a pagination query. All API calls with the following request parameter name support pagination queries. Please see the following:

| Parameters	| 描述|
|----------|--------|
| pageIndex	| Current page|
| pageSize	| The maximum number of strings on each page is 100|

 
## Standard specification
* Timestamp
Unless otherwise specified, all timestamps in the API are returned in milliseconds.

* Data Accuracy
In order to maintain the data integrity and precision across platforms, decimal data are returned as character strings. It is recommended that you also convert numbers to character strings when making requests to avoid truncation and precision errors.


 
## Interface type

* Public interfaces

Public interfaces can be used to get configuration information and ticker data. Public requests can be invoked without authentication.

* Private interfaces

Private interfaces can be used for order management and account management. Each private request must be validated with a signature generated by your private key.
You can generate an API key here: Account Settings ->My API -> Create API keys; 

Link Address: `https://www.idax.pro/myapi`.



 
## Rate limits

API call rate limits
* API call rate limit <= 5 times/ sec from the same IP;
* API call rate limit <= 5 times/sec from the same key

 
## Signature validation

How to get the request signature?

All parameters submitted by the user, except the ‘sign’parameter, will be signed to get the signature. All parameter names are named in the camel case and the first letter of the parameter names are in lowercase. The parameter names to be signed are sorted alphabetically (by comparing the first letters of all parameter names alphabetically or the second letter if the first letters are the same, and the like).

For instance, you want to place an order, the serialized string is: 

`amount=1.05&key=123456789&orderSide=buy&orderType=limit&pair=ETH_BTC&price=0.069249& timestamp=1532522823039`

Finally, the HmacSha256 algorithm is used to get the signature with your private key and the above string. The obtained hexadecimal code is assigned to the request parameter ‘sign’.


## Trading pairs

IDAX Public Trading Pairs, We will make changes on an occasional basis

|   |   |   |
|----|----| ----|
| AE/BTC	| ADN/ETH	| BAW/USDT |
| BCHABC/BTC| 	AE/ETH	| BK/USDT |
| BST/BTC	| DDPC/ETH	| BNB/USDT |
| BSV/BTC	| EOS/ETH	| BTC/USDT |
| BTS/BTC	| LINKA/ETH	| BTE/USDT |
| DASH/BTC	| MACH/ETH	| BTT/USDT |
| EOS/BTC	| NULS/ETH	| CSAC/USDT |
| ETH/BTC	| OMG/ETH	| CUST/USDT |
| FK/BTC	| PKO/ETH	| DFT/USDT |
| GNA/BTC	| SNT/ETH	| DOGE/USDT |
| GNY/BTC	| STORJ/ETH	| EST/USDT |
| IOST/BTC	| TRX/ETH	| ETC/USDT |
| KUV/BTC	| UGC/ETH	| ETH/USDT |
| LINKA/BTC	| XD/ETH	| FK/USDT |