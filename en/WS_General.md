# General

### SSL Websocket Connection

> Pro: wss://api.huobi.pro/ws

> HADAX: wss://api.hadax.com/ws

### Data

All return data of websocket APIs needs to be unzipped.

Recommend: [pako](https://github.com/nodeca/pako)

### Library

Recommend: [ws](https://github.com/websockets/ws) by Node.js

### Topic

Requesting and subscribing to the data need to use the `topic`

| type      | topic                     |Operation| description                                       |Authentication Required|
| ------------- | ---------------------------- | --------------|---------------------------------------- |-----|
| KLine         | market.$symbol.kline.$period | sub, unsub| $period ：{ 1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year } |N|
| Market Depth  | market.$symbol.depth.$type   |  sub, unsub| $type ：{ step0, step1, step2, step3, step4, step5 } （depths 0-5）|N|
| Trade Detail  | market.$symbol.trade.detail  |  sub, unsub|                                          |N|
| Market Detail | market.$symbol.detail        |  sub, unsub|                                          |N|
| Market Tickers | market.tickers        | sub, unsub| |N|
| Accounts update |accounts | sub, unsub | Subscribe,unsubscribe from all asset change messages for a given user |Y|
| Order updates |orders.$symbol | sub, unsub | Subscribing to or unsubscribing to an order change message. When the $symbol is `*`, it means to subscribe to all trading symbols. | Y|
|Account data request|accounts.list | req | Request account list|Y|
|Order list request|orders.list | req | Request Order list | Y|
|Order detail request|orders.detail | req | Order details query request|Y|

`$symbol` ： { ethbtc, ltcbtc, etcbtc, bccbtc ... }


### Heartbeat

```
//WebSocket Client Request
{
    "ping": 18212558000
}

//WebSocket Server response
{
    "pong": 18212558000
}
```

If the type of request message is not `LONG`, websocket server will response:

```
{
  "ts": 1492420473027,
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid ping"
}
```

### Request


```
//request
{
  "req": "topic to req",
  "id": "id generate by client"
}
```
`"req"` use the **topic**,as above "Topic"

**correct example**

```
//request
{
  "req": "market.ethbtc.kline.1min",
  "id": "id10"
}
```


```
//response
{
  "status": "ok",
  "rep": "market.btccny.kline.1min",
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

**error request**

```
//request
{
  "req": "market.invalidsymbo.kline.1min",
  "id": "id10"
}
```

```
//response
{
  "status": "error",
  "id": "id10",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.invalidsymbol.trade.detail",
  "ts": 1494483996521
}
```


### Subscribe

To receive data you have to send a "sub" message first.

```
//request
{
  "sub": "topic to sub",
  "id": "id generate by client"
}
```

**correct sample**


```
//request
{
  "sub": "market.btccny.kline.1min",
  "id": "id1"
}
```

`"req"` use the **topic**,as above "Topic".

```
//response
{
  "id": "id1",
  "status": "ok",
  "subbed": "market.btccny.kline.1min",
  "ts": 1489474081631
}
```

After subscribe,you will receive updates upon any change.

```
//example
{
  "ch": "market.btccny.kline.1min",
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


**error example**

wrong symbol


```
//response
{
  "id": "id2",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.invalidsymbol.kline.1min",
  "ts": 1494301904959
}
```

wrong topic

```
//response
{
  "id": "id3",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "invalid topic market.btccny.kline.3min",
  "ts": 1494310283622
}
```

### unsubscribe

To stop receiving data from a channel you have to send a "unsubscribe" message.

```
//request
{
  "unsub": "topic to unsub",
  "id": "id generate by client"
}
```

**correct sample**

```
//request
{
  "unsub": "market.btccny.trade.detail",
  "id": "id4"
}
```

```
//response
{
  "id": "id4",
  "status": "ok",
  "unsubbed": "market.btccny.trade.detail",
  "ts" 1494326028889
}
```

**error unsubscribe**

Unsubscribe an unsubscribed topic

```
//response
{
  "id": "id5",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "unsub with not subbed topic market.btccny.trade.detail",
  "ts": 1494326217428
}
```

Unsubscribe a nonexistent topic


```
//response
{
  "id": "id5",
  "status": "error",
  "err-code": "bad-request",
  "err-msg": "unsub with not subbed topic not-exists-topic",
  "ts": 1494326318809
}
```
### Authentication
For the topics require Authentication, please follow the same signature generation rules as indicated in [Signing API Request](https://github.com/huobiapi/API_Docs_en/wiki/Signing_API_Requests), where AccessKey is the API
access key, and SecretKey is the key for users to sign the request.

*important note：These two keys are closely related to account security
and should not be disclosed to others at any time。*

In the version of trade Websocket , for the topics require authentication, server must do Authentication before
making connection ,the format is:

**The format of Authentication Example**

    {
      "op": "auth",
      "AccessKeyId": "e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx", 
      "SignatureMethod": "HmacSHA256",
      "SignatureVersion": "2",
      "Timestamp": "2017-05-11T15:19:30",
      "Signature": "4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o="
    }

**The format of Authentication data instruction**

  |filed              |type   |  instruction|
  |------------------ |--------| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |op                 |string|   required;the type of requested operator is auth;|
  |cid                |string   |optional;the ID of Client request|
  |AccessKeyId        |string   |required;API access key , AccessKey is in APIKEY you applied|
  |SignatureMethod    |string   |required;the method of sign, user computes signature basing on the protocol of hash ,the api uses HmacSHA256|
  |SignatureVersion   |string   |required;the version of signature's protocol ,the api uses 2
  |Timestamp          |string   |required;timestamp, the time is you requests (UTC timezone) 。this value is to avoid that another people intercepts your request。for example ：2017-05-11T16:22:06。 (UTC timezone)|
  |Signature          |string   |required;signature, the value is computed to make sure that the Authentication is valid and not tampered|

> **Notice：**
>
> -   Parameter `op`,`cid`, `Signature` **Do not participate in operations**
> -   The request method in signature's method is `GET`,the other parameter reference REST api document








