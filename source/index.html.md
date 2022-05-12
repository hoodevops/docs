---
title: Hoo API 文档

language_tabs: # must be one of https://git.io/vQNgJ
- shell

search: False
---

# 简介

## API 简介

欢迎使用Hoo API！ 你可以使用此 API 获得市场行情数据，进行交易，并且管理你的账户。

在文档的右侧是代码，目前我们仅提供针对 `shell` 的代码示例。

交易市场分为币币交易和创新区交易。

可以使用以下域名访问：api.hoolgd.com

欢迎有优秀 maker 策略且交易量大的机构参与长期做市商项目。

##  钱包账户接口
可以访问的接口如下：

接口|说明|
----------------------|---------------------|---------------------|
[GET /api/pub/v1/coins](#91b7b9fcc6)  |获取币种信息接口|
[GET /api/pub/v1/addresses](#98cff3e8b0)  |新增获取充值地址|
[GET /api/pub/v1/deposit/bills](#e2a027ceb6)  |获取充值记录|
[POST /api/pub/v1/withdraw](#da0ff3f199)  |用户提币接口|
[GET /api/pub/v1/withdraw/bills](#dc0a5f2bd0)  |获取用户提币记录|
[GET /api/pub/v1/account](#121b7ff6b1)  |获取账户状态|


## 公共接口
不需要鉴权可访问接口如下：

接口|说明|市场|
----------------------|---------------------|---------------------|
[GET /open/v1/tickers/market](#495cebdeec-2)  |所有交易对|币币|
[GET /open/v1/depth/market](#a1128a972d-2)  |深度|币币|
[GET /open/v1/trade/market](#775841b581-2)  |逐笔成交|币币|
[GET /open/v1/kline/market](#k-3)  |k线数据|币币|
[GET /open/innovate/v1/tickers/market](#495cebdeec-2)  |所有交易对|创新区|
[GET /open/innovate/v1/depth/market](#a1128a972d-2)  |深度|创新区|
[GET /open/innovate/v1/trade/market](#775841b581-2)  |逐笔成交|创新区|
[GET /open/innovate/v1/kline/market](#k-3)  |k线数据|创新区|


## 鉴权接口
可以访问的接口如下：

接口|说明|市场|
----------------------|---------------------|---------------------|
[GET /open/v1/tickers](#ebe64e52ff)  |全部或指定交易对|币币|
[GET /open/v1/balance](#870c0ab88b)  |获取余额|币币|
[GET /open/v1/timestamp](#fc5a31ea39)  |服务器时间戳|币币|
[GET /open/v1/kline](#k-2)  |市场k线数据|币币|
[GET /open/v1/depth](#0f7bd4961a)  |市场深度数据|币币|
[GET /open/v1/tickers/trade](#5)  |获取最近5条成交记录|币币|
[POST /open/v1/orders/place](#fd6ce2a756)  |下单|币币|
[POST /open/v1/orders/cancel](#7742416be6)  |撤销单个订单|币币|
[POST /open/v1/orders/batcancel](#cedb99e805)  |撤销全部或部分委托中订单|币币|
[GET /open/v1/orders/last](#c2313ec9bf)  |委托中列表|币币|
[GET /open/v1/orders](#cbbcc98be2)  |订单列表|币币|
[GET /open/v1/orders/detail](#3fbc9cb788)  |单个订单成交明细|币币|
[GET /open/v1/orders/detailmore](#d1baf83d74)  |分页获取成交明细|币币|
[GET /open/v1/orders/fee-rate](#6033256dc0)  |获取用户某个交易对手续费|币币|
[GET /open/innovate/v1/tickers](#ebe64e52ff)  |全部或指定交易对|创新区|
[GET /open/innovate/v1/balance](#870c0ab88b)  |获取余额|创新区|
[GET /open/innovate/v1/timestamp](#fc5a31ea39)  |服务器时间戳|创新区|
[GET /open/innovate/v1/kline](#k-2)  |市场k线数据|创新区|
[GET /open/innovate/v1/depth](#0f7bd4961a)  |市场深度数据|创新区|
[GET /open/innovate/v1/tickers/trade](#5)  |获取最近5条成交记录|创新区|
[POST /open/innovate/v1/orders/place](#fd6ce2a756)  |下单|创新区|
[POST /open/innovate/v1/orders/cancel](#7742416be6)  |撤销单个订单|创新区|
[POST /open/innovate/v1/orders/batcancel](#cedb99e805)  |撤销全部或部分委托中订单|创新区|
[GET /open/innovate/v1/orders/last](#c2313ec9bf)  |委托中列表|创新区|
[GET /open/innovate/v1/orders](#cbbcc98be2)  |订单列表|创新区|
[GET /open/innovate/v1/orders/detail](#3fbc9cb788)  |单个订单成交明细|创新区|
[GET /open/innovate/v1/orders/detailmore](#d1baf83d74)  |分页获取成交明细|创新区|
[GET /open/innovate/v1/orders/fee-rate ](#6033256dc0) |获取用户某个交易对手续费|创新区|


# 接入说明

## Restful host:
    https://api.hoolgd.com

## Websocket host:
    币币交易
    wss://api.hoolgd.com/ws

    创新区交易
    wss://api.hoolgd.com/wsi

## 鉴权说明


1. 所有接口都需要进行鉴权，参数为client_id, ts, nonce, sign。client_id是api key, client_key为密钥，请妥善保管。

2. client_id为api key，ts为当前时间戳，与服务器时间差正负5秒会被拒绝，nonce为随机字符串，不能与上次请求所使用相同。

3. 签名方法, 将client_id, ts, nonce进行排序连接，使用hmac-sha256方法进行签名，例如待签名字符串为: client_id=abc&nonce=xyz&ts=1571293029

4. 签名: sign = hmac.New(client_key, sign_str, sha256)

5. Content-Type: application/x-www-form-urlencoded

6. 币币/创新区接口，post接口请求请将参数放在请求体里面，get接口请求携带在url链接中。

7. 钱包账户接口，请将鉴权信息放在header头部信息，HOO-KEY = api key，Hoo-Sign = sign，Hoo-Timestamp = ts。

8. 每个api key都与ip进行绑定，最多可设置5个ip。


# 钱包账户接口

## 获取币种信息接口

此接口获取HOO平台币种

```shell

"https://api.hoolgd.com/api/pub/v1/coins"

```

### HTTP请求
- GET `/api/pub/v1/coins`

<aside class="notice">限速10r/m</aside>

### 请求参数
无

> Responds:

```json
{
  "code": "10000",
  "data": {
    "coins": [
      {
        "coin_name": "BSV",
        "coin_id": 225,
        "chain_list": [
          {
            "fee_unit": "BSV",
            "min_amount": "0",
            "fee": "396.54557133",
            "max_amount": "10000",
            "is_withdraw": 0,
            "is_accept": 0,
            "chain_name": "BSV"
          }
        ],
        "coin_icon": "https:\/\/api.hoolgd.com\/media\/news_thumb\/ab02d7f9-0732-4623-8f8d-b7a2dff96cf7.jpg"
      }
    ],
    "count": 1
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|count|int|币种数量|
|coins|array|币种详情[{|
|coin_name|string|币种名称|
|coin_id|string|币种ID|
|chain_list|array|币种支持的链[{|
|fee_unit|string|手续费币种名称|
|min_amount|integer|最小提币额|
|fee|integer|手续费数量|
|max_amount|string|最大提币额|
|is_withdraw|string|是否允许提币|
|is_accept|string|是否允许充值|
|chain_name|string|链名称}]|
|coin_icon|string|币种图标}]|

## 新增获取充值地址

此接口获取用户币种充值地址

```shell

"https://api.hoolgd.com/api/pub/v1/addresses"

```

### HTTP请求
- GET `/api/pub/v1/addresses`

<aside class="notice">限速10r/m</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|coin_name|string|是|币种名称，如: BTC|
|chain_name|string|否|链名称，如: BTC|

> Responds:

```json
{
  "code": "10000",
  "data": {
    "addresses": [
      {
        "address": "",
        "coin_name": "USDT",
        "chain_name": "OMNI"
      }
    ],
    "count": 1
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|addresses|array|币种不同链的地址[{|
|address|string|地址， 可能为空字符串，表示目前还未分配有地址|
|coin_name|string|币种名称|
|chain_name|string|链名称}]|
|count|int|数量|

## 获取充值记录

此接口获取用户充值记录

```shell

"https://api.hoolgd.com/api/pub/v1/deposit/bills"

```

### HTTP请求
- GET `/api/pub/v1/deposit/bills`

<aside class="notice">限速10r/m</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|coin_name|string|是|币种名称，如: BTC|
|status|int|否|状态 1成功 2进行中 3失败 不传或传其他查全部|
|bill_uuid|string|否|账单流水号|
|start_at|string|否|开始时间戳 秒级|
|end_at|string|否|截止时间戳 秒级|
|page|string|否|页码 默认1|
|pagesize|string|否|每页条数 默认20|

> Responds:

```json
{
  "code": "10000",
  "data": {
    "count": 1,
    "pagesize": 20,
    "bills": [
      {
        "transaction_id": "",
        "amount": "80",
        "side": 1,
        "address": "",
        "coin_name": "USDT",
        "bill_uuid": "20220322114814949011120867",
        "burn_amount": "",
        "process_time": 1647920894,
        "chain_name": "ERC20",
        "status": 1
      }
    ],
    "page": 1
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|count|int|账单数量|
|bills|array|账单[{|
|bill_uuid|string|账单流水号|
|status|string|状态 1成功 2进行中 3失败|
|process_time|string|处理完成时间戳 秒级|
|chain_name|string|链名称|
|amount|string|数量|
|address|string|接收地址|
|transaction_id|string|交易哈希|
|side|string|站内站内 1站内 2站外|
|burn_amount|string|燃烧数量|
|coin_name|string|币种名称}]|
|page|int|页码|
|pagesize|int|每条条数|

## 用户提币接口

此接口用于用户提币

```shell

"https://api.hoolgd.com/api/pub/v1/withdraw"

```

### HTTP请求
- POST `/api/pub/v1/withdraw`

<aside class="notice">限速10r/m</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|coin_name|string|是|币种名称，如: BTC|
|chain_name|string|是|链名称，如: BTC|
|amount|string|是|数量|
|to_address|string|是|接收地址|
|memo|string|是|接收地址memo|

> Responds:

```json
{
  "code": "10000",
  "data": {
    "order_no": "20220426152343066472638058",
    "coin_name": "USDT",
    "chain_name": "BSC"
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|order_no|string|账单流水号|
|coin_name|string|币种名称|
|chain_name|string|链名称|

## 获取用户提币记录

此接口获取用户提币记录

```shell

"https://api.hoolgd.com/api/pub/v1/withdraw/bills"

```

### HTTP请求
- GET `/api/pub/v1/withdraw/bills`

<aside class="notice">限速10r/m</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|coin_name|string|是|币种名称，如: BTC|
|status|int|否|状态 1成功2进行中3失败|
|bill_uuid|string|否|账单流水号|
|start_at|string|否|开始时间戳 秒级|
|end_at|string|否|截止时间戳 秒级|
|page|string|否|页码 默认1|
|pagesize|string|否|每页条数 默认20|

> Responds:

```json
{
  "code": "10000",
  "data": {
    "count": 1,
    "pagesize": 20,
    "bills": [
      {
        "amount": "22",
        "side": 1,
        "fee_unit": "USDT",
        "fee": "0",
        "to_address": "",
        "transaction_list": [
        ],
        "process_time": 1650957823,
        "coin_name": "USDT",
        "bill_uuid": "20220426152343066472638058",
        "create_at": 1650957823,
        "from_address": "",
        "chain_name": "BSC",
        "status": 2
      },
    ],
    "page": 1
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|count|int|账单数量|
|bills|array|账单[{|
|bill_uuid|string|账单流水号|
|status|string|状态 1成功 2进行中 3失败|
|process_time|string|处理完成时间戳 秒级|
|create_at|string|生成时间戳 秒级|
|chain_name|string|链名称|
|amount|string|数量|
|to_address|string|接收地址|
|side|string|站内站内 1站内 2站外|
|coin_name|string|币种名称|
|from_address|string|发送地址|
|transaction_list|array|交易上链列表[{|
|index_no|int|交易序号|
|amount|string|数量|
|transaction_id|string|交易哈希|
|confirmations|int|确认数|
|total_confirmations|int|总的确认数	|
|block_url|string|路由|
|block_id|int|交易区块}]|
|fee|string|手续费|
|fee_unit|string|手续费币种}]|
|page|int|页码|
|pagesize|int|每条条数|

## 获取账户状态

此接口获取账户状态

```shell

"https://api.hoolgd.com/api/pub/v1/account"

```

### HTTP请求
- POST `/api/pub/v1/account`

<aside class="notice">限速10r/m</aside>

### 请求参数
无

> Responds:

```json
{
  "code": "10000",
  "data": {
    "coin_name": "BTC",
    "is_withdraw": 1,
    "withdraw_left": "0.99948384",
    "withdraw_total": 1
  },
  "message": "success"
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|withdraw_total|string|24h总的提币额|
|withdraw_left|string|24h剩余提币额|
|coin_name|string|币种名称|
|is_withdraw|string|是否允许提币|


# WebSocket说明

1. 需要先进行鉴权，才可进行订阅。

2. 鉴权格式: {"op":"apilogin","sign":"","client_id":"","nonce":"","ts": int type},如:{"op":"apilogin","sign":"abc123","client_id":"abc123","nonce":"1","ts": 1576207749}

3. 心跳处理，客户端需定时上发心跳信息，任意字符串，服务端每30秒会检查心跳，超时没有收到自动关闭连接。{"op":"sub", "topic":"hb"}

## 订阅主题
    {"op":"sub", "topic": ""}

### K线数据
#### 请求参数

```json
{"op":"sub", "topic": "kline:1Min:EOS-USDT"}
```

|参数|说明|
|:---:|:---:|
|kline:1Min:EOS-USDT|EOS-USDT的1分钟k线|

> Responds:

```json
{
    "symbol":"EOS-USDT",
    "ticks":[
        {
            "close":"2.62",
            "high":"3.11",
            "low":"2.62",
            "open":"3.01",
            "timestamp":1572851100,
            "volume":"17.55"
        }
    ],
    "timestamp":1572851160917,
    "topic":"kline:1Min:EOS-USDT",
    "type":"60000"
}
```

#### 数据更新字段列表
| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|symbol|string|交易对|
|ticks|object|k线信息|
|close|string|本阶段收盘价|
|high|string|本阶段最高价|
|low|string|本阶段最低价|
|open|string|本阶段开盘价
|timestamp|integer|时间戳 毫秒|
|volume|string|成交量|

### 逐笔成交
#### 请求参数

```json
{"op":"sub", "topic": "trade:LTC-USDT"}
```

|参数|说明|
|:---:|:---:|
|trade:LTC-USDT|LTC-USDT逐笔成交记录|

> Responds:

```json
{
    "amount":"7.473",
    "price":"2.82",
    "side":1,
    "symbol":"EOS-USDT",
    "timestamp":1572851197910,
    "topic":"trade:EOS-USDT",
    "volume":"2.65"
}
```

#### 数据更新字段列表

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|price|string|成交价|
|side|integer|成交方向，1买，-1卖|
|symbol|string|交易对|
|timestamp|integer|时间戳 毫秒|
|volume|string|成交量|

### 深度
#### 请求参数

```json
{"op":"sub", "topic": "depth:0:LTC-USDT"}
```

|参数|说明|
|:---:|:---:|
|depth:0:LTC-USDT|深度|

> Responds:

```json
{
    "bids":[
        {'price': '2.923', 'quantity': '12'},
        {'price': '2.823', 'quantity': '12'},
        {'price': '2.723', 'quantity': '16'}
    ], 
    "asks":[
        {'price': '3.05', 'quantity': '3.48'},
        {'price': '3.31', 'quantity': '5'},
        {'price': '3.55', 'quantity': '10'}
    ],
    "symbol":"EOS-USDT",
    "timestamp":1572851208935,
    "topic":"depth:0:EOS-USDT"
}
```

#### 数据更新字段列表

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|bids|object|当前所有买单[{price 价格, quantity 数量}]|
|asks|object|当前所有卖单[{price 价格, quantity 数量}]|

### 行情
#### 请求参数

```json
{"op":"sub", "topic": "quotes"}
```

|参数|说明|
|:---:|:---:|
|quotes|行情|

> Responds:

```json
{
    "amount":"52080.1255",
    "change":"0.00949367",
    "price":"3.19",
    "symbol":"EOS-USDT",
    "timestamp":1572851216950,
    "topic":"quotes",
    "volume":"17965.65"
}
```
#### 数据更新字段列表

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|change|string|涨跌幅|
|price|string|当前价|
|symbol|string|交易对|
|timestamp|integer|时间戳 毫秒|
|volume|string|24小时成交量|

### 账户余额变化
#### 请求参数

```json
{"op":"sub", "topic": "accounts"}
```

|参数|说明|
|:---:|:---:|
|accounts|账户余额变化|

> Responds:

```json
{
    "available":"4194.3466678",
    "freeze":"71.609185",
    "symbol":"USDT",
    "topic":"accounts",
    "total":"4265.9558528"
}
```

#### 数据更新字段列表

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|available|string|可用余额|
|freeze|string|冻结余额|
|symbol|string|币种|
|total|string|总余额|

### 委托变化
#### 请求参数

```json
{"op":"sub", "topic": "orders:BTC-USDT"}
```

|参数|说明|
|:---:|:---:|
|orders:BTC-USDT|委托变化|

> Responds:

```json
// 下单
{
    "left":"1",
    "order_id":"11574948935833473",
    "order_type":1,
    "price":"80000",
    "quantity":"1",
    "side":-1,
    "status":2,
    "symbol":"BTC-USDT",
    "timestamp":1574949805841,
    "topic":"orders:BTC-USDT",
    "trade_no":"499081745280826070655",
    "match_qty":"0"
}

// 撤单
{
    "left":"0",
    "order_id":"11574948935833473",
    "order_type":1,
    "price":"80000",
    "quantity":"1",
    "side":-1,
    "status":6,
    "symbol":"BTC-USDT",
    "timestamp":1574949805841,
    "topic":"orders:BTC-USDT",
    "trade_no":"499081745280826070655",
    "match_qty":"0",
    "match_price":"0"
}
```

#### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|left|string|剩余数量|
|order_id|string|订单id|
|order_type|int|订单类型,1限价，3市价|
|price|string|委托价|
|quantity|string|委托数量|
|side|int|方向，1买，-1卖|
|status|int|状态 2 委托中，3部分成交，4全部成交，5部分成交后撤消，6全部撤消|
|symbol|string|交易对|
|timestamp|int|创建时间 毫秒|
|trade_no|string|订单流水号|
|match_qty|string|已成交数量|
|match_price|string|成交均价|


# 基础信息

## 所有交易对
此接口返回全部或指定hoo支持的交易对。

```shell
币币市场

"https://api.hoolgd.com/open/v1/tickers"

创新区市场

"https://api.hoolgd.com/open/innovate/v1/tickers"
```

### HTTP请求
币币市场
- GET ` /open/v1/tickers`

创新区市场
- GET ` /open/innovate/v1/tickers`

<aside class="notice">限速1r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|否|交易对，如: BTC-USDT|

> Responds:

```json
{  
    'code': 0, 
    'data': [
            {'amount': '1.586',
            'change': '-0.235462',
            'high': '3.05',
            'low': '3.05',
            'price': '0',
            'symbol': 'EOS-USDT',
            'amt_num': 4,
            'qty_num': 2,
            'volume': '0.52'
            }, 
        ]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|24小时成交额|
|change|string|24小时涨跌幅|
|high|string|24小时最高|
|low|string|24小时最低|
|price|string|当前价|
|symbol|string|交易对|
|amt_num|integer|价格精度|
|qty_num|integer|数量精度|
|volume|string|24小时成交量|

## 账户余额

```shell
币币市场

"https://api.hoolgd.com/open/v1/balance"

创新区市场

"https://api.hoolgd.com/open/innovate/v1/balance"
```

### HTTP请求
币币交易
- GET ` /open/v1/balance`

创新区交易
- GET ` /open/innovate/v1/balance`

### 请求参数
无

> Responds:

```json
{
    'code': 0, 
    'data': [
        {
            'amount': '4317.6696678', 
            'symbol': "USDT", 
            'freeze': '71.609185'
        },
    ]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|可用余额|
|symbol|string|币种|
|freeze|string|冻结余额|

## 服务器时间戳

```shell
币币交易

"https://api.hoolgd.com/open/v1/timestamp"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/timestamp"
```

### HTTP请求
币币交易
- GET ` /open/v1/timestamp`

创新区交易
- GET ` /open/innovate/v1/timestamp`

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
无
> Responds:

```json
{
    "code": 0,
    "msg": "ok",
    "data": "12354534",
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|data|string|时间戳|


# 行情数据

## 市场k线数据

```shell
币币交易

"https://api.hoolgd.com/open/v1/kline"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/kline"
```

### HTTP请求
币币交易
- GET ` /open/v1/kline`

创新区交易
- GET ` /open/innovate/v1/kline`

<aside class="notice">限速0.1r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对，如: BTC-USDT|
|type|string|是|类型1Min, 5Min, 15Min, 30Min等|

> Responds:

```json
{
    'code': 0, 
    'data': [
        {
            'amount': '0',
            'close': '3.05',    
            'high': '3.05', 
            'low': '3.05', 
            'open': '3.05', 
            'time': 1571812440, 
            'volume': '0'
        }
    ]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|close|string|本阶段收盘价|
|high|string|本阶段最高价|
|low|string|本阶段最低价|
|open|string|本阶段开盘价
|time|integer|时间|
|volume|string|成交量|

## 市场深度数据

```shell
币币交易

"https://api.hoolgd.com/open/v1/depth"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/depth"
```

### HTTP请求
币币交易
- GET ` /open/v1/depth`

创新区交易
- GET ` /open/innovate/v1/depth`

<aside class="notice">限速10r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|

> Responds:

```json
{
    'code': 0, 
    'data': {
        'bids': [
            {'price': '2.923', 'quantity': '12'}, 
            {'price': '2.823', 'quantity': '12'}, 
            {'price': '2.813', 'quantity': '14'}
        ], 
        'asks': [
            {'price': '3.05', 'quantity': '3.48'}, 
            {'price': '3.31', 'quantity': '15'}, 
            {'price': '3.923', 'quantity': '15'}
            ]
        }, 
    'msg': 'ok'
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|bids|object|当前所有买单[{price 价格, quantity 数量}]|
|asks|object|当前所有卖单[{price 价格, quantity 数量}]|

## 获取最近5条成交

```shell
币币交易

"https://api.hoolgd.com/open/v1/tickers/trade"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/tickers/trade"
```

### HTTP请求
币币交易
- GET ` /open/v1/tickers/trade`

创新区交易
- GET ` /open/innovate/v1/tickers/trade`

<aside class="notice">限速10r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|

> Responds:

```json
{
    'code': 0, 
    'data': [{
        'amount': '0.918',
        'price': '2.04',
        'side': -1,
        'time': 1574942822160,
        'volume': '0.45'
        }]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|price|string|成交价|
|side|integer|成交方向，1买，-1卖|
|time|integer|时间|
|volume|string|成交量|


# 现货

## 下单

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/place"

创新区交易

"https://api.hoolgd.com/innovate/open/v1/orders/place"
```

### HTTP请求
币币交易
- POST ` /open/v1/orders/place`

创新区交易
- POST ` /open/innovate/v1/orders/place`

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|
|price|string|是|价格|
|quantity|string|是|数量|
|side|int|是|方向,1买，-1卖|

<aside class="warning">无论买或卖，quantity都表示交易币，如EOS-USDT，quantity都代表eos的数量</aside>
<aside class="warning">创新区新上交易对第一笔订单必须通过此接口下单，成交后用户才能下单</aside>

> Responds:

```json
{
    "code": 0,
    "msg": "ok",
    "data": {
        "order_id": "xxx",
        "trade_no": "xxx",
    },
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|order_id|string|委托号|
|trade_no|string|流水号|

## 撤销单个订单

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/cancel"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders/cancel"
```

### HTTP请求
币币交易
- POST ` /open/v1/orders/cancel`

创新区交易
- POST ` /open/innovate/v1/orders/cancel`

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|
|order_id|string|是|委托号|
|trade_no|string|是|流水号|

> Responds:

```json
{
    "code": 0,
    "msg": "ok",
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
无

## 撤销部分或所有委托中订单

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/batcancel"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders/batcancel"
```

### HTTP请求
币币交易
- POST ` /open/v1/orders/batcancel`
  创新区交易
- POST ` /open/innovate/v1/orders/batcancel`

<aside class="notice">限速1r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|
|order_ids|string|否|交易对id，1000,2000,3000， 英文逗号分隔订单id，为空全部撤单|

> Responds:

```json
{
    "code": 0,
    "msg": "ok",
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
无

## 委托中列表

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/last"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders/last"
```

### HTTP请求
币币交易
- GET ` /open/v1/orders/last`

创新区交易
- GET ` /open/innovate/v1/orders/last`

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|

> Responds:

```json
{
    'code': 0, 
    'data': [
        {
            'symbol': 'BTC-USDT',
            'order_id': '11574744030837944',
            'trade_no': '499016576021202015341',
            'price': '7900',
            'quantity': '1',
            'match_amt': '0',
            'match_qty': '0',
            'match_price': '',
            'side': -1,
            'order_type': 1,
            'create_at': 1574744151836
        }, 
    ], 
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|symbol|string|交易对
|order_id|string|订单ID|
|trade_no|string|订单流水号|
|price|string|委托价|
|quantity|string|委托数量|
|match_amt|string|已成交金额|
|match_qty|string|已成交数量|
|match_price|string|成交均价|
|side|int|方向，1买，-1卖|
|order_type|int|订单类型,1限价，3市价|
|create_at|int|创建时间|

## 订单列表

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders"
```

### HTTP请求
币币交易
- GET ` /open/v1/orders`

创新区交易
- GET ` /open/innovate/v1/orders`

<aside class="notice">限速0.5r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对,如EOS-USDT|
|pagenum|int|否|页码|
|pagesize|int|否|页大小,最小10, 最大50,默认20|
|side|int|否|方向，1买，-1卖，0所有|
|start|int|否|时间，时间戳|
|end|int|否|结束时间，时间戳|

> Responds:

```json
{
    'code': 0, 
    'msg': 'ok',
    'data': {
        'count': 4, 
        'orders': [
            {
                'order_id': '11574744030837944',
                'trade_no': '499016576021202015341',
                'symbol': 'BTC-USDT',
                'price': '7900',
                'quantity': '1',
                'match_amt': '0',
                'match_qty': '0',
                'match_price': '',
                'side': -1,
                'order_type': 1,
                'status': 6,
                'create_at': 1574744151836
            }, 
        ]
    }, 
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|order_id|string|订单id|
|trade_no|string|订单流水号|
|symbol|string|交易对|
|price|string|委托价|
|quantity|string|委托数量|
|match_amt|string|已成交金额|
|match_qty|string|已成交数量|
|match_price|string|成交均价|
|side|int|方向，1买，-1卖|
|order_type|int|订单类型,1限价，3市价|
|status|int|状态 2 委托中，3部分成交，4全部成交，5部分成交后撤消，6全部撤消|
|create_at|int|创建时间|

## 单个订单成交明细

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/detail"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders/detail"
```

### HTTP请求
币币交易
- GET ` /open/v1/orders/detail`

创新区交易
- GET ` /open/innovate/v1/orders/detail`

<aside class="notice">限速6r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对,如EOS-USDT|
|order_id|string|是|委托订单id|

> Responds:

```json
{
    'code': 0, 
    'data': {
        'order_id': '11574751725833010',
        'trade_no': '499073202290421221116', 
        'symbol': 'BTC-USDT', 
        'price': '70000', 
        'quantity': '0.0001', 
        'match_amt': '7', 
        'match_qty': '0.0001',
        'match_price': '70000',  
        'fee': '0.0112',
        'side': -1, 
        'order_type': 1,
        'status': 4,
        'create_at': 1574922846832,
        'trades': [{
            'trade_id': "1",
            'amount': '7', 
            'price': '70000', 
            'quantity': '0.0001',
            'fee': '0.0112',  
            'time': 1574922846833
            }]
    }
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|order_id|string|订单id|
|trade_no|string|订单流水号|
|symbol|string|交易对|
|price|string|委托价|
|quantity|string|委托数量|
|match_amt|string|已成交金额|
|match_qty|string|已成交数量|
|match_price|string|成交均价|
|fee|string|手续费|
|side|int|方向，1买，-1卖|
|order_type|int|订单类型,1限价，3市价|
|status|int|状态 2 委托中，3部分成交，4全部成交，5部分成交后撤消，6全部撤消|
|create_at|int|委托单创建时间|
|trades|object|已成交数据[{|
|trade_id|string|成交记录id|
|amount|string|每条成交记录的成交额|
|price|string|每条成交记录的成交价|
|quantity|string|每条成交记录的成交量|
|fee|string|每条成交记录的手续费|
|time|int|每条成交记录的成交时间}]|

## 分页获取订单成交明细

```shell
币币交易

"https://api.hoolgd.com/open/v1/orders/detailmore"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/orders/detailmore"
```

### HTTP请求
币币交易
- GET ` /open/v1/orders/detailmore`

创新区交易
- GET ` /open/innovate/v1/orders/detailmore`

<aside class="notice">限速6r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对,如EOS-USDT|
|pagesize|int|否|页大小,最小10, 最大50,默认10|
|pagenum|int|否|页码，默认为1|

> Responds:

```json
{
    'code': 0, 
    'data': {
        'count': 10,
        'trades': [
            {
              'amount': '11574751725833010',
              'fee': '499073202290421221116',
              'symbol': 'BTC-USDT',
              'price': '70000',
              'quantity': '0.0001',
              'side': '7',
              'time': 1574922846833,
              'trade_id': 1,
            }
          ]
    }
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|count|int|成交订单数量|
|trades|object|已成交数据[{|
|symbol|string|交易对|
|side|int|方向，1买，-1卖|
|trade_id|int|成交记录id|
|amount|string|每条成交记录的成交额|
|price|string|每条成交记录的成交价|
|quantity|string|每条成交记录的成交量|
|fee|string|每条成交记录的手续费|
|time|int|每条成交记录的成交时间}]|

## 获取用户某个交易对手续费

```shell
币币市场

"https://api.hoolgd.com/open/v1/fee-rate"

创新区市场

"https://api.hoolgd.com/open/innovate/v1/fee-rate"
```

### HTTP请求
币币交易
- GET ` /open/v1/fee-rate`

创新区交易
- GET ` /open/innovate/v1/fee-rate`

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对|

> Responds:

```json
{
    'code': 0, 
    'data': {
        'maker_fee': '0.0001',
        'taker_fee': "0.0002"
    }
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|maker_fee|string|挂单手续费|
|taker_fee|string|吃单手续费|


# 公共接口

## 所有交易对

```shell
币币交易

"https://api.hoolgd.com/open/v1/tickers/market"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/tickers/market"
```

### HTTP请求
币币交易
- GET ` /open/v1/tickers/market`

创新区交易
- GET ` /open/innovate/v1/tickers/market`

<aside class="notice">限速6r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|

> Responds:

```json
{  
    'code': 0, 
    'data': [
            {'amount': '1.586',
            'change': '-0.235462',
            'high': '3.05',
            'low': '3.05',
            'price': '0',
            'symbol': 'EOS-USDT',
            'amt_num': 4,
            'qty_num': 2,
            'volume': '0.52'
            }, 
        ]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|24小时成交额|
|change|string|24小时涨跌幅|
|high|string|24小时最高|
|low|string|24小时最低|
|price|string|当前价|
|symbol|string|交易对|
|amt_num|integer|价格精度|
|qty_num|integer|数量精度|
|volume|string|24小时成交量|

## 深度

```shell
币币交易

"https://api.hoolgd.com/open/v1/depth/market"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/depth/market"
```

### HTTP请求
币币交易
- GET ` /open/v1/depth/market`

创新区交易
- GET ` /open/innovate/v1/depth/market`

<aside class="notice">限速5r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:
|symbol|string|是|交易对名称,如BTC-USDT|

> Responds:

```json
{
    'code': 0, 
    'data': {
        'bids': [
            {'price': '2.923', 'quantity': '12'}, 
            {'price': '2.823', 'quantity': '12'}, 
            {'price': '2.813', 'quantity': '14'}
        ], 
        'asks': [
            {'price': '3.05', 'quantity': '3.48'}, 
            {'price': '3.31', 'quantity': '15'}, 
            {'price': '3.923', 'quantity': '15'}
            ]
        }, 
    'msg': 'ok'
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|bids|object|当前所有买单[{price 价格, quantity 数量}]|
|asks|object|当前所有卖单[{price 价格, quantity 数量}]|

## 逐笔成交

```shell
币币交易

"https://api.hoolgd.com/open/v1/trade/market"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/trade/market"
```

### HTTP请求
币币交易
- GET ` /open/v1/trade/market`

创新区交易
- GET ` /open/innovate/v1/trade/market`

<aside class="notice">限速10r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对名称,如BTC-USDT|

> Responds:

```json
{
    'code': 0, 
    'data': [{
        'amount': '0.918',
        'price': '2.04',
        'side': -1,
        'time': 1574942822160,
        'volume': '0.45'
        }]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|price|string|成交价|
|side|integer|成交方向，1买，-1卖|
|time|integer|时间|
|volume|string|成交量|

## k线数据

```shell
币币交易

"https://api.hoolgd.com/open/v1/kline/market"

创新区交易

"https://api.hoolgd.com/open/innovate/v1/kline/market"
```

### HTTP请求
币币交易
- GET ` /open/v1/kline/market`

创新区交易
- GET ` /open/innovate/v1/kline/market`

<aside class="notice">限速1r/s</aside>

### 请求参数
|参数名|参数类型|是否必须|描述|
|:---:|:---:|:---:|:---:|
|symbol|string|是|交易对，如: BTC-USDT|
|type|string|是|类型1Min, 5Min, 15Min, 30Min等|

> Responds:

```json
{
    'code': 0, 
    'data': [
        {
            'amount': '0',
            'close': '3.05',    
            'high': '3.05', 
            'low': '3.05', 
            'open': '3.05', 
            'time': 1571812440, 
            'volume': '0'
        }
    ]
}
```

### 返回字段

| 参数名 | 参数类型 | 描述 |
|:-----:|:------:|:----:|
|amount|string|成交额|
|close|string|本阶段收盘价|
|high|string|本阶段最高价|
|low|string|本阶段最低价|
|open|string|本阶段开盘价
|time|integer|时间|
|volume|string|成交量|


# 钱包账户API调用示例

> Python:

```python
# -*- coding:utf-8 -*-

import hmac
import json
from urllib.parse import urlencode

import requests
import time
import typing as t


class Hoo:
    endpoint = "https://api.hoolgd.com"
    session = requests.session()

    HEADER_SIGNATURE = 'HOO-SIGN'
    HEADER_TIMESTAMP = 'HOO-TIMESTAMP'
    HEADER_APIKEY = 'HOO-KEY'

    POST = 'POST'
    GET = 'GET'

    def __init__(self, apikey: str, secret: str, endpoint=None):
        self.apikey = apikey
        self.secret = secret
        if endpoint:
            self.endpoint = endpoint

    def sign(self, timestamp: int, method, path, data=None):
        payload = f'{timestamp}{self.apikey}{method}{path}'
        if data is not None:
            if method == self.GET:
                payload += urlencode(data, doseq=True)
            elif method == self.POST:
                payload += json.dumps(data)
        return hmac.new(self.secret.encode('utf8'), payload.encode('utf8'), digestmod='sha256').hexdigest()

    def request(self, method: str, path: str, data=None):
        return self.dispatch(method)(self.endpoint + path, **{'params' if method == self.GET else 'json': data}).json()

    def request_signed(self, method: str, path: str, data=None):
        timestamp = int(time.time() * 1e3)
        signature = self.sign(timestamp, method.upper(), path, data=data)
        return self.dispatch(method)(self.endpoint + path, **{
            'headers': {
                self.HEADER_APIKEY: self.apikey,
                self.HEADER_TIMESTAMP: str(timestamp),
                self.HEADER_SIGNATURE: signature,
            },
            'params' if method == self.GET else 'json': data
        }).json()

    def dispatch(self, method: str):
        return {
            'POST': self.session.post,
            'GET': self.session.get
        }[method.upper()]

    def coins(self):
        return self.request_signed(self.GET, '/api/pub/v1/coins')

    def addresses(self, coin_name: str = "", chain_name: str = ""):
        return self.request_signed(self.GET, '/api/pub/v1/addresses', data={
            "coin_name": coin_name,
            "chain_name": chain_name,
        })

    def deposit_bills(self, coin_name: str, status: int = None, bill_uuid: str = None, start_at: int = None,
                        end_at: int = None, page: int = None, pagesize: int = None):
        return self.request_signed(self.GET, '/api/pub/v1/deposit/bills', data={
            'coin_name': coin_name,
            'status': status if status else 0,
            'bill_uuid': bill_uuid if bill_uuid else "",
            'start_at': start_at if start_at else 0,
            'end_at': end_at if end_at else 0,
            'page': page if page else 1,
            'pagesize': pagesize if pagesize else 20
        })

    def withdraw(self, coin_name: str, chain_name: str, amount: str, to_address: str, memo: str):
        return self.request_signed(self.POST, '/api/pub/v1/withdraw', data={
            'coin_name': coin_name,
            'chain_name': chain_name,
            'amount': amount,
            'to_address': to_address,
            'memo': memo
        })

    def withdraw_bills(self, coin_name: str, status: int = None, bill_uuid: str = None, start_at: int = None,
                        end_at: int = None, page: int = None, pagesize: int = None):
        return self.request_signed(self.GET, '/api/pub/v1/withdraw/bills', data={
            'coin_name': coin_name,
            'status': status if status else '',
            'bill_uuid': bill_uuid if bill_uuid else 0,
            'start_at': start_at if start_at else 0,
            'end_at': end_at if end_at else 0,
            'page': page if page else 1,
            'pagesize': pagesize if pagesize else 20
        })

    def account(self):
        return self.request_signed(self.GET, '/api/pub/v1/account')

def print_json(v):
    print(json.dumps(v, indent=2))


def main():
    apikey = 'xxxxx'
    secret = 'xxxxx'
    hoo = Hoo(apikey=apikey, secret=secret)
    print_json(hoo.coins())


if __name__ == '__main__':
    main()

```


# 现货API调用示例

> Python:

```python
# -*- coding:utf-8 -*-

import requests
import time
import hmac
import hashlib
import ujson
import random

host = "https://api.hoolgd.com"
client_id = ""
client_key = ""

def gen_sign(client_id, client_key):
    ts = int(time.time())
    nonce = "abcdefg"
    obj = {"ts": ts, "nonce": nonce, "sign": "", "client_id": client_id}
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts) 
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj["sign"] = v.hexdigest()
    return obj 

print("> 获取open委托中")
# 币币交易
path = "/open/v1/orders/last"
# 创新区交易
# path = "/open/innovate/v1/orders/last"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "ETH-USDT"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 获取单个订单成交明细")
# 币币交易
path = "/open/v1/orders/detail"
# 创新区交易
# path = "/open/innovate/v1/orders/detail"
obj = gen_sign(client_id, client_key)
obj.update({"order_id": "11574751725833010", "symbol": "BTC-USDT"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 分页获取成交明细")
# 币币交易
path = "/open/v1/orders/detailmore"
# 创新区交易
# path = "/open/innovate/v1/orders/detailmore"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "BTC-USDT", "pagesize": 10, "pagenum": 1"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 获取用户某个交易对手续费")
# 币币交易
path = "/open/v1/orders/fee-rate"
# 创新区交易
# path = "/open/innovate/v1/orders/fee-rate"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "BTC-USDT"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 获取kline")
# 币币交易
path = "/open/v1/kline"
# 创新区交易
# path = "/open/innovate/v1/kline"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "EOS-USDT", "type": "1Min"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 获取余额")
# 币币交易
path = "/open/v1/balance"
# 创新区交易
# path = "/open/innovate/v1/balance"
obj = gen_sign(client_id, client_key)
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 获取最近成交记录")
# 币币交易
path = "/open/v1/tickers/trade"
# 创新区交易
# path = "/open/innovate/v1/tickers/trade"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "EOS-USDT"})
res = requests.get(host + path, params=obj)
print(ujson.loads(res.content))

print("> 下单")
# 币币交易
path = "/open/v1/orders/place"
# 创新区交易
# path = "/open/innovate/v1/orders/place"
obj = gen_sign(client_id, client_key)
obj.update({"symbol": "BTC-USDT", "price": "8850.21", "quantity": "0.1", "side": "1"})
res = requests.post(host + path, data=obj)
print(ujson.loads(res.content))
```

# Websocket示例

```python
# -*- coding:utf-8 -*-

import time
import hmac
import hashlib
import websockets
import json
import asyncio
import uvloop

asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())

# 币币交易
host = "wss://api.hoolgd.com/ws"
# 创新区交易
# host = "wss://api.hoolgd.com/wsi"
client_id = ""
client_key = ""

def login():
    ts = int(time.time())
    nonce = "abcdefg"
    obj = {"ts": ts, "nonce": nonce, "sign": "", "client_id": client_id, "op": "apilogin"}
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts)
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj["sign"] = v.hexdigest()
    return obj

async def sub_topic(ws):
    sub = "depth:0:EOS-USDT"
    await ws.send(json.dumps({"op": "sub", "topic": sub}))

async def startup():
    print("start to connect %s..." % host)
    ws = await websockets.connect(host)

    obj = login()
    await ws.send(json.dumps(obj))
    await sub_topic(ws)

    while 1:
        try:
            data = await ws.recv()
            print(data)
        except websockets.exceptions.ConnectionClosed as e:
            print("connect closed...", e)
            return
        except:
            pass

if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(startup())
```

# HooSwap

[HooSwap示例说明](https://github.com/hoodoc/doc)
