# 请求与订阅说明


### 1. 访问地址

**注意：<br>**
**_1.请使用中国大陆以外的服务器访问火币 API；_<br>**

- Global 站行情请求地址为：wss://api.huobi.pro/ws
- Global 站资产和订单推送统一服务地址为：wss://api.huobi.pro/ws/v1
- 合约站行情请求以及订阅地址为：wss://www.hbdm.com/ws

### 2. 数据压缩
WebSocket API 返回的所有数据都进行了 GZIP 压缩，需要 client 在收到数据之后解压，推荐使用pako。（[【pako】](https://github.com/nodeca/pako) 是一个支持压缩和解压 GZIP 的库）

### 3. WebSocket库
[【ws】](https://github.com/websockets/ws) 是 Node.js 下的 WebSocket 库。

### 4. 心跳
WebSocket API 支持双向心跳，无论是 Server 还是 Client 都可以发起 `ping` message，对方返回 `pong` message。

WebSocket Server 发送心跳：
```
{"ping": 18212558000}
```
 WebSocket Client 应该返回：
```
 {"pong": 18212558000}
```
注：返回的数据里面的 `"pong"` 的值为收到的 `"ping"` 的值
注：WebSocket Client 和 WebSocket Server 建立连接之后，WebSocket Server 每隔 `5s`（这个频率可能会变化） 会向 WebSocket Client 发起一次心跳，WebSocket Client 忽略心跳2次后，WebSocket Server 将会主动断开连接；WebSocket Client发送最近2次心跳message中的其中一个`ping`的值，WebSocket Server都会保持WebSocket连接。
```
┌────────┐                         ┌────────┐ 
│ Client │                         │ Server │
└───┬────┘                         └───┬────┘
    │         {"ping": 18212558000}  │
    │<─────────────────────────────────┤
    │                                  │ wait 5s
    │                                  ├───┐
    │                                  │<──┘
    │         {"ping": 18212558000}  │
    │<─────────────────────────────────┤
    │                                  │
    │ {"pong": 18212558000}          │
    ├┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄>│
    │                                  │
```
注：WebSocket Client 发送最近2次心跳 message 中的其中一个 `"ping"` 的值，WebSocket Server 都会保持 WebSocket 连接
```
┌────────┐                         ┌────────┐ 
│ Client │                         │ Server │
└───┬────┘                         └───┬────┘
    │         {"ping": 1523778470416}  │
    │<─────────────────────────────────┤
    │                                  │ wait 5s
    │                                  ├───┐
    │                                  │<──┘
    │         {"ping": 1523778475416}  │
    │<─────────────────────────────────┤
    │                                  │ wait 5s
    │                                  ├───┐
    │                                  │<──┘
    │                                  │
    │                                  │ close WebSocket connection
    │                                  ├───┐
    │                                  │<──┘
    │                                  │

```
注：WebSocket Client 忽略心跳2次后，WebSocket Server 将会主动断开连接。
WebSocket Client 发送心跳：
```
{"ping": 18212553000}
```
注：发送的 message 里面，"ping" 的值必须为 Long 类型，否则返回错误信息：
```
{
  "ts": 1492420473027,
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid ping"
}
```

WebSocket Server 会返回：
```
{"pong": 18212553000}
```
注：返回的数据里面的 `"pong"` 的值为收到的 `"ping"` 的值

错误信息返回格式
```
{
  "id": "id generate by client",
  "status": "error",
  "err-code": "err-code",
  "err-msg": "err-message",
  "ts": 1487152091345
}
```
注：`ts`为错误信息生成的时间戳，单位：毫秒
### 5. topic格式

订阅数据和请求数据都要使用 `topic`，`topic` 的语法如下：

| topic 类型      | topic 语法                    |sub/req     | 描述                                       |是否需要验签|
| ------------- | ---------------------------- | ---------------|------------------------- |-----|
| KLine         | market.$symbol.kline.$period | sub/req |K线 数据，包含单位时间区间的开盘价、收盘价、最高价、最低价、成交量、成交额、成交笔数等数据 $period 可选值：{ 1min, 5min, 15min, 30min, 60min, 4hour,1day, 1mon, 1week, 1year } |N|
| Market Depth  | market.$symbol.depth.$type   | sub/req |盘口深度，按照不同 step 聚合的买一、买二、买三等和卖一、卖二、卖三等数据 $type 可选值：{ step0, step1, step2, step3, step4, step5, percent10 } （合并深度0-5）；step0时，不合并深度|N|
| Trade Detail  | market.$symbol.trade.detail  |  sub/req |  成交记录，包含成交价格、成交量、成交方向等信息                                      |N|
| Market Detail | market.$symbol.detail        |  sub/req |   最近24小时成交量、成交额、开盘价、收盘价、最高价、最低价、成交笔数等                                     |N|
| Market Tickers | market.tickers  |  sub|   所有对外公开交易对的 日K线、最近24小时成交量等信息                            |N|
|Accounts|accounts|sub, unsub|订阅账户资产变更|Y|
|Orders|orders.$symbol|sub,unsub|订阅订单变更|Y|
|Accounts list|accounts.list|req|请求账户资产信息|Y|
|Order list|accounts.list|req|请求订单信息|Y|
|order detail|orders.detail|req|请求某个订单明细|Y|

* `$symbol` 为交易对，可选值： { ethbtc, ltcbtc, etcbtc, bchbtc...... }
* 用户选择“合并深度”时，一定报价精度内的市场挂单将予以合并显示。合并深度仅改变显示方式，不改变实际成交价格。

### 6. 请求数据(req/rep)

请求数据，仅返回一次数据

#### 请求数据的格式

```
{
  "req": "topic to req",
  "id": "id generate by client"
}
```
* `"req"` 的值为 **topic** ，请参考 `"5. topic格式"` 的 **topic 格式**

**正确请求数据的例子**

```
{
  "req": "market.btcusdt.kline.1min",
  "id": "id10"
}
```

返回数据的例子：
```
{
  "status": "ok",
  "rep": "market.btcusdt.kline.1min",
  "tick": [
    {
      "amount": 1.6206,
      "count":  3,
      "id":     1494465840,
      "open":   9887.00,
      "close":  9885.00,
      "low":    9885.00,
      "high":   9887.00,
      "vol":    16021.632026
    },
    {
      "amount": 2.2124,
      "count":  6,
      "id":     1494465900,
      "open":   9885.00,
      "close":  9880.00,
      "low":    9880.00,
      "high":   9885.00,
      "vol":    21859.023500
    }
  ]
}
```

**错误请求数据的例子**

```
{
  "req": "market.invalidsymbo.kline.1min",
  "id": "id10"
}
```

返回的错误信息的例子：
```
{
  "status": "error",
  "id": "id10",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.invalidsymbol.trade.detail",
  "ts": 1494483996521
}
```


### 7. 订阅数据(sub)

#### 订阅数据(sub)以及接收订阅数据的大致流程
```
┌────────┐                         ┌────────┐ 
│ Client │                         │ Server │
└───┬────┘                         └───┬────┘
    │ {"sub": "topic"}                 │
    ├─────────────────────────────────>│
    │                                  │
    │              {"subbed": "topic"} │
    │<┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┤
    │                                  │
    │        {"tick": "data of topic"} │
    │<┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┤
    │                                  │
    │        {"tick": "data of topic"} │
    │<┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┤
    │                                  │
```
注：订阅 topic 成功之后，当 topic 对应的数据有更新时，Server 按一定的频率把 topic 对应的新数据推送给 Client
#### 订阅数据的格式

成功建立和 WebSocket API 的连接之后，向 Server 发送如下格式的数据来订阅数据：
```
{
   "id": "id generate by client",
  "sub": "topic to sub",
  "freq-ms": 1000
}
```
注：`id` 参数是可选的
注：`freq-ms` 参数是可选的，可选值为 { `1000`, `2000`, `3000`, `4000`, `5000` }，不传 `freq-ms` 时默认为` 0 `也就是有新的数据就马上推送到 Client，`freq-ms` 决定 Server 推送的频率
**正确订阅的例子**

正确订阅：
```
{
  "sub": "market.btcusdt.kline.1min",
  "id": "id1"
}
```
* `"sub"` 的值为 **topic** ，请参考 `"5. topic格式"` 的 **topic 格式**

订阅成功返回数据的例子：
```
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btcusdt.kline.1min",
  "ts": 1489474081631
}
```

之后每当 KLine 有更新时，client 会收到数据，例子：
```
{
  "ch": "market.btcusdt.kline.1min",
  "ts": 1489474082831,
  "tick": {
    "id": 1489464480,
    "amount": 0.0,
    "count": 0,
    "open": 7962.62,
    "close": 7962.62,
    "low": 7962.62,
    "high": 7962.62,
    "vol": 0.0
  }
}
```
tick 说明: 

```
  "tick": {
    "id": K线id,
    "amount": 成交量,
    "count": 成交笔数,
    "open": 开盘价,
    "close": 收盘价,当K线为最晚的一根时，是最新成交价
    "low": 最低价,
    "high": 最高价,
    "vol": 成交额, 即 sum(每一笔成交价 * 该笔的成交量)
  }

```


**错误订阅的例子**

错误订阅（错误的 symbol）：
```
{
  "sub": "market.invalidsymbol.kline.1min",
  "id": "id2"
}
```

订阅失败返回数据的例子：
```
{
  "id": "id2",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.invalidsymbol.kline.1min",
  "ts": 1494301904959
}
```

错误订阅（错误的 topic）：
```
{
  "sub": "market.btcusdt.kline.3min",
  "id": "id3"
}
```
订阅失败返回数据的例子：
```
{
  "id": "id3",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.btcusdt.kline.3min",
  "ts": 1494310283622
}
```

### 8. 取消订阅(unsub)

#### 取消订阅的格式

WebSocket Client 订阅数据之后，可以取消订阅，取消订阅之后 WebSocket Server 将不会再发送该 topic 的数据，取消订阅的格式如下：

```
{
  "unsub": "topic to unsub",
  "id": "id generate by client"
}
```

**正确取消订阅的例子**

正确取消订阅的例子：
```
{
  "unsub": "market.btcusdt.trade.detail",
  "id": "id4"
}
```

取消订阅成功返回信息的例子：
```
{
  "id": "id4",
  "status": "ok",
  "unsubbed": "market.btcusdt.trade.detail",
  "ts" 1494326028889
}
```

**错误取消订阅的例子**

错误取消订阅的例子（取消订阅一个尚未订阅的 topic）：
```
{
  "unsub": "market.btcusdt.trade.detail",
  "id": "id5"
}
```

返回的错误信息的例子
```
{
  "id": "id5",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "unsub with not subbed topic market.btcusdt.trade.detail",
  "ts": 1494326217428
}
```

错误取消订阅的例子（取消订阅一个不存在的 topic）：
```
{
  "unsub": "not-exists-topic",
  "id": "id5"
}
```
返回的错误信息的例子：
```
{
  "id": "id5",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "unsub with not subbed topic not-exists-topic",
  "ts": 1494326318809
}
```
### 9. 鉴权（Authentication）

用户自己在火币网生成Access Key和Secret Key，Secret
Key由用户自己保存，用户需提供Access Key。目前关于 apikey
申请和修改，请在“账户 - API 管理 ” 创建新API Key
填写备注（可选择绑定ip）点击创建。其中 Access Key 为 API
访问密钥，Secret Key
为用户对请求进行签名的密钥（仅申请时可见）。用户按规则生成签名(Signature)。

交易功能 websocket 版本接口建立连接时首先要做鉴权操作，具体格式如下，

*重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露。*

**鉴权请求数据格式**

    {
      "op": "auth",
      "AccessKeyId": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx", 
      "SignatureMethod": "HmacSHA256",
      "SignatureVersion": "2",
      "Timestamp": "2017-05-11T15:19:30",
      "Signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=",
    }

**鉴权请求数据格式说明**

 | 字段名称           |类型     |说明|
 | ------------------| --------| --------------------------------------------------|
 | op                | string   |必填；操作名称，鉴权固定值为 auth；|
  |cid               | string  | 选填；Client 请求唯一 ID|
  |AccessKeyId        |string   |必填；API 访问密钥, 您申请的 APIKEY 中的 AccessKey|
  |SignatureMethod    |string   |必填；签名方法, 用户计算签名的基于哈希的协议，此处使用 HmacSHA256|
  |SignatureVersion   |string   |必填；签名协议的版本，此处使用 2|
  |Timestamp          |string   |必填；时间戳, 您发出请求的时间 (UTC 时区) (UTC 时区) (UTC 时区) 。在查询请求中包含此值有助于防止第三方截取您的请求。如：2017-05-11T16:22:06。再次强调是 (UTC 时区)|
| Signature          |string   |必填；签名, 计算得出的值，用于确保签名有效和未被篡改|

> **注意：**
>
> -   为了减少已有用户的接入工作量，此处使用了与 REST
>     接口同样的签名算法进行鉴权，详细请参考
>     [REST\_authentication\#签名运算](https://github.com/huobiapi/API_Docs/wiki/REST_authentication#%E7%AD%BE%E5%90%8D%E8%BF%90%E7%AE%97)。
> -   请注意大小写
> -   参数 `op`，`cid`, `Signature` **不参加签名计算**
> -   此处签名计算中请求方法固定值为`GET`，其余值请参考 REST
>     接口签名算法文档

步骤：

示例参数签名（Signature）计算过程如下，

-   规范要计算签名的请求 因为使用 HMAC
    进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。

-   请求方法，后面添加换行符`\n`。

<!-- -->

    GET\n

-   添加小写的访问地址，后面添加换行符 `\n`

<!-- -->

    api.huobipro.com\n

-   访问方法的路径，后面添加换行符 `\n`。

<!-- -->

    /ws/v1\n

-   按照ASCII码的顺序对参数名进行排序(使用 UTF-8 编码，且进行了 URI
    编码，十六进制字符必须大写，如‘:’会被编码为'%3A'，空格被编码为'%20')。
    例如，下面是请求参数的原始顺序，进行过编码后。

<!-- -->

    AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&Signature
    Method=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%
    3A19%3A30

-   按照以上顺序，将各参数使用字符’&’连接。
    -   组成最终的要进行签名计算的字符串如下：
        -   计算签名，将以下两个参数传入加密哈希函数：
            要进行签名计算的字符串，进行签名的密钥（SecretKey）
        -   得到签名计算结果并进行 Base64编码
-   将上述值作为参数Signature的取值添加到 API 请求中。

**鉴权应答数据格式说明**

|  名称       |类型      |说明                                                 |
|  ---------- |--------- |----------------------------------------------------|
|  op         |string    |必填；操作名称，鉴权固定值为 auth |
|  err-code   |integer   |成功返回 0, 失败为其他值，详细响应码列表请参考附录|
|  err-msg   | string    |可选，若出错表示详细错误信息|
 | ts        | long      |服务端应答时间戳|
 |user-id    |long      |用户 id|

鉴权成功应答数据示例

    {
      "op": "auth",
      "ts": 1489474081631,
      "err-code": 0,
      "data": {
        "user-id": 1000
      }
    }

鉴权失败应答返回数据

    {
      "op": "auth",
      "ts": 1489474081631,
      "err-code": 1010，
      "err-msg": ”详细的错误信息“
    }

若用户上一次鉴权还未应答，用户连续发送鉴权请求，会返回重复鉴权的错误应答：

    {
      "op": "auth",
      "ts": 1489474081631,
      "err-code": 1022，
      "err-msg": ”详细的错误信息“
    }