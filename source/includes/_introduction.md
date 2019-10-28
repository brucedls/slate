# API概述

为用户提供一整套简单而又强大的开发工具，旨在帮助用户快速、高效地将IDAX行情和交易功能整合到自己的应用当中。 IDAX接口是提供服务的基础，开发者在IDAX网站创建账号后，可以根据自身需求建立不同权限的API，并利用API进行自动交易。 通过API可以快速实现以下功能：获取接口权限后，可以通过阅读本接口文档来帮助开发。如果您在使用过程中遇到任何问题，请联系我们，将在第一时间为您做出解答。

## 撮合引擎

### 交易对（pair）

表现形式：`XXX_YYY`，其中`XXX`是基准币，`YYY`是报价币，譬如：`ETH_BTC`交易对，`ETH`是基准币，`BTC`是报价币。

### 买卖方向

* 买入: buy
* 卖出: sell

### 订单类型（orderType : 限价:limit; 市价:market）

限价交易：指对买卖的价格作出限定，即在买入基准币时，限定一个最高价；在卖出基准币时，则限定一个最低价。限价交易除限定价格外，还须限定交易基准币的数量。

市价交易：指对买卖的价格不做限定，即按当前的市场价格买入或卖出，市价买单指令须限定买入报价币的金额；市价卖单指令须限定卖出基准币的数量。

### 订单模式（OrderMode : 标准模式:0; 止损:10; 止盈:11; 移动止损:20）

标准模式：默认的订单模式。

限价止损单：一种在市场达到特定价格后，以设定价格卖出或平仓的订单，用于控制损失。

限价止盈单：类似于限价止损单，同样在市场达到特定价格后，以设定价格卖出或平仓的订单，用于控制盈利。

市价止损单：一种在市场达到特定价格后，以市价卖出或平仓的订单，用于控制损失。

市价止盈单：类似于市价止损单，同样在市场达到特定价格后，以市价卖出或平仓的订单，用于控制盈利。

移动止损单：当市场价格到达触发价，需要设置一个距离触发价的价格差，当价格回调到设置的价格差后，移动止损单被触发，并发出市价委托。移动止损卖出订单可以在币价上升时保护和最大化利润，以及在币价下跌时限制亏损。

### 订单属性（OrderProperty : 标准属性:0; FAK:1; FOK:2; POST-ONLY:3）：

NORMAL：默认的订单属性。

FAK：立即成交，剩余自动撤销。仅支持限价、限价止损、限价止盈。

FOK：立即全部成交或全部撤销。仅支持限价、限价止损、限价止盈。

POST-ONLY：如果委托无法全部进入深度列表，则全部撤销。仅支持限价、限价止损、限价止盈。

### 规则

撮合规则：IDAX撮合引擎撮合规则：价格优先、时间优先，即：按照价格优于时间的优先级来撮合，价格相同时按照下单时间顺序撮合，先下单的先撮合。

成交价规则：订单撮合时成交价将按maker挂单价格成交，而非taker吃单价格。

### 订单生命周期

1、订单进入撮合引擎后是"未成交"状态；

2、一个订单被撮合可能出现部分成交，那么他的状态会变成"部分成交"状态，并且继续留在待撮合队列中等待撮合；

3、如果一个订单被撮合而全部成交，那么它会变成"已成交"状态；

4、一个留在撮合队伍中等待撮合的订单被撤销，那么他的状态会变成"已撤销"状态，"已撤销"状态订单此前可能有"部分成交"；

被撤销或者全部成交的订单将被从撮合队列中剔除。

## 费用

费率：`https://www.idax.pro/fee`

 
## 服务器

为了最大限度地减少API访问延迟，建议您使用香港的服务器。

 
## 请求

### REST API 

访问的根URL： `http://openapi.idax.pro/api/v2`

所有请求基于https协议，请求头中Content Type须统一设置为:`application/json`。

请求交互说明(REST 服务的返回值，均包含了code和msg)：

1、封装请求参数：根据接口请求参数规定进行参数封装。

2、提交请求参数：将封装好的请求参数通过POST或GET方式提交至服务器。

3、服务器响应：服务器将对用户请求数据进行参数安全校验，通过校验后将根据业务逻辑将响应数据以JSON格式返回给用户。

4、响应数据处理：用户对服务器响应数据按需进行处理。

### WebSocket API

#### 连接地址 

`wss://openws.idax.pro/ws`

 
#### 请求数据格式

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
event: addChannel(注册请求数据)/removeChannel(注销请求数据)/clearChannel(注销所有请求)
channel：IDAX 提供请求数据类型
parameters: 参数为选填参数，预留用于交易服务,其中`apiKey` 为用户申请到的API Key `sign` 为签名字符串，签名规则参照请求说明
 
#### 单个注册
`websocket.send("{'event':'addChannel','channel':'idax_sub_eth_btc_ticker'}")`

 
#### 批量注册
`websocket.send("[ {'event':'addChannel','channel':'idax_sub_eth_btc_ticker'}, {'event':'addChannel','channel':'idax_sub_eth_btc_depth'}, {'event':'addChannel','channel':'idax_sub_eth_btc_trades'} ]");`

 
#### 服务器响应

返回数据格式为：

`[{"channel":"channel","code":"","data":[{}]}, {"channel":"channel","code":"","data":[{}]}]`

* channel:请求的数据类型
* code: "00000": 注册/取消注册success，其他注册/取消注册failure
* data：返回结果数据
 
#### 推送过程说明

* 为保证推送的及时性及减少流量，行情数据（ticker）和委托深度（depth）这两种数据类型只会在数据发生变化的情况下才会推送数据，交易记录（trades）是推送从上次推送到本次推送产生的增量数据。
* 每次订阅成功，都会推送一次数据，data里面包含当前的数据，code表示是否订阅成功
* 每次取消订阅，服务器也会推送一条code为成功的消息
* idax_sub_X_depth_Y每秒推送1次，每次全量推送

 
## 分页
部分API提供了分页查询，带有以下请求字段的API均可分页查询，参数如下：

| 参数名	| 描述|
|----------|--------|
| pageIndex	| 当前页数|
| pageSize	| 每页条数，最多100条|

 
## 标准规范
* 时间戳
除非另外指定，API中的所有时间戳均以毫秒为单位返回。

* 数字精度
为了保持跨平台时精度的完整性，十进制数字作为字符串返回。建议您在发起请求时也将数字转换为字符串以避免截断和精度错误。


 
## 接口类型
* 公共接口
公共接口可用于获取配置信息和行情数据。公共请求无需认证即可调用。

* 私有接口
私有接口可用于订单管理和账户管理。每个私有请求必须使用规范的验证形式进行签名。私有接口需要使用您的API key进行验证。

您可以在这里生成API key：账户设置->我的API->创建API密钥；链接地址： `https://www.idax.pro/myapi`。


 
## 访问限制

每个接口都限制了请求频次: 

* 不需要验签的接口，同一IP请求频率 <=5次/sec；
* 需要验签的接口，同一key请求频率 <=5次/sec

 
## 验证签名

如何对请求参数进行签名 用户提交的参数除sign参数以外，都必须要参与签名，所有参数名采用驼峰命名，参数名首字母小写。 将待签名字符串按照参数名进行排序（比较所有参数名的第一个字母，按abcd顺序排列，若遇到相同首字母，则看第二个字母，以此类推）。

以【下单交易】的字符串为例，排序后的字符串： 

`amount=1.05&key=123456789&orderSide=buy&orderType=limit&pair=ETH_BTC&price=0.069249& timestamp=1532522823039`

最后，是利用HmacSha256算法，对最终待签名字符串进行签名运算，将得到的字节数组采用16进制码，从而得到签名结果的字符串，该字符串赋值于请求参数sign。

## 交易对

IDAX 公开交易对，将不定期调整

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