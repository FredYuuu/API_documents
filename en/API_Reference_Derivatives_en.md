API List
--------

| API Type  | **Content Type** | **Context**                                      | **Request Type** | **Desc**                                       | **Signature Required** |
| --------- | ---------------- | ------------------------------------------------ | ---------------- | ---------------------------------------------- | ---------------------- |
| Restful   | Market Data      | <a href="#1">/v1/contract_contract_info </a>     | GET              | Get Contracts Information                      | No                     |
| Restful   | Market Data      | <a href="#2">/v1/contract_index </a>             | GET              | Get contract Index Price Information           | No                     |
| Restful   | Market Data      | <a href="#3"> /v1/contract_price_limit </a>      | GET              | Get Contract Price Limits                      | No                     |
| Restful   | Market Data      | <a href="#4"> /v1/contract_open_interest</a>     | GET              | Get Contract Open Interest Information         | No                     |
| Restful   | Market Data      | <a href="#5"> /market/depth </a>                 | GET              | Get Market Depth                               | No                     |
| Restful   | Market Data      | <a href="#6">/market/history/kline  </a>         | GET              | Get K-Line Data                                | No                     |
| Restful   | Market Data      | <a href="#7"> /market/detail/merged </a>         | GET              | Get Market Data Overview                       | No                     |
| Restful   | Market Data      | <a href="#8"> /market/trade </a>                 | GET              | The Last Trade of a Contract                   | No                     |
| Restful   | Market Data      | <a href="#9">/market/history/trade  </a>         | GET              | Request a Batch of Trade Records of a Contract | No                     |
| Restful   | Account          | <a href="#101"> /v1/contract_account_info </a>   | POST             | User’s Account Information                     | Yes                    |
| Restful   | Account          | <a href="#102">/v1/contract_position_info  </a>  | POST             | User’s position Information                    | Yes                    |
| Restful   | Trade            | <a href="#103"> /v1/contract_order </a>          | POST             | Place an Order                                 | Yes                    |
| Restful   | Trade            | <a href="#104">/v1/contract_batchorder   </a>    | POST             | Place a Batch of Orders                        | Yes                    |
| Restful   | Trade            | <a href="#105">/v1/contract_cancel </a>          | POST             | Cancel an Order                                | Yes                    |
| Restful   | Trade            | <a href="#106">/v1/contract_cancelall  </a>      | POST             | Cancel All Orders                              | Yes                    |
| Restful   | User Order Info  | <a href="#107">/v1/contract_order_info  </a>     | POST             | Get Information of an Order                    | Yes                    |
| Restful   | User Order Info  | <a href="#108"> /v1/contract_order_detail  </a>  | POST             | Get Trade Details of an Order                  | Yes                    |
| Restful   | User Order Info  | <a href="#109"> /v1/contract_openorders  </a>    | POST             | Get Current Orders                             | Yes                    |
| Restful   | User Order Info  | <a href="#110"> /v1/contract_hisorders  </a>     | POST             | Get History Orders                             | Yes                    |



Signature Authentication
--------

#### <a name="199">Authentication Method</a>

User private interface (account, trade and user order info), all interfaces related to user need to be encipherment verification，to verify the user’s legality.
Authentication method is the same as pro spot goods，please refer to : <https://github.com/huobiapi/API_Docs/wiki/REST_authentication>


#### <a name="200">Access Times Limitation</a>

Both the public market interface and the user private interface have access times limitation. 

**Public market interface**: 10 times/second. 

**User private interface**: 5 times/second.





Restful API
===========


Market Data
--------

#### <a ame="1"> Get Contract Info  </a> 

URL /v1/contract_contract_info

**Request Parameter**

| **Parameter Name** | **Type** | **Mandatory** | **Description** |
| ------------------ | -------- | ------------- | --------------- |
| symbol             | string   | false         | "BTC","ETH"...  |
| contract_type | string   | false      | "this_week","next_week", "quarter" |
| contract_code | string   | false      | BTC180914|

**Note**：
Note：If there is a number in the Contract Code row，inquiry with Contract_Code. If there is no number，inquiry by Symbol + Contract Type. One of the query conditions must be chosen.

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Description**                               | **Value Range**                                              |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | ------------------------------------------------------------ |
| status                         | true          | string   | Request Processing Result                     | "ok" , "error"                                               |
| \<list\>(Attribute Name: data) |               |          |                                               |                                                              |
| symbol                         | true          | string   | Product Code                                  | "BTC","ETH"...                                               |
| contract_code                  | true          | string   | Contract Code                                 | "BTC180914" ...                                              |
| contract_type                  | true          | string   | Contract Type                                 | "this_week","next_week", "quarter"                           |
| contract_size                  | true          | decimal  | Contract Value (USD of one contract)          | 10, 100...                                                   |
| price_tick                     | true          | decimal  | Minimum Variation of Contract Price           | 0.001, 0.01...                                               |
| delivery_date                  | true          | string   | Contract Delivery Date                        | eg "20180720"                                                |
| create_date                    | true          | string   | Contract Listing Date                         | eg "20180706"                                                |
| contract_status                | true          | int      | Contract Status                               | 0: Delisting,1: Listing,2: Pending Listing,3: Suspension,4: Suspending of Listing,5: In Settlement,6: Delivering,7: Settlement Completed,8: Delivered,9: Suspended Listing |
| \</list\>                      |               |          |                                               |                                                              |
| ts                             | true          | long     | Time of Respond Generation，Unit：Millisecond |                                                              |

**Example**
```
GET https://api.hbdm.com/api/v1/contract_contract_info

# Response
{
  "status": "ok",
  "data": [
    {
      "symbol": "BTC",
      "contract_code": "BTC180914",
      "contract_type": "this_week",
      "contract_size": 100,
      "price_tick": 0.001,
      "delivery_date": "20180704",
      "create_date": "20180604",
      "contract_status": 1
     }
    ],
  "ts":158797866555
}

```
#### <a name="2">Get Contract Index Price Information  </a>

URL  /v1/contract_index

**Request Parameter**

| **Parameter Name** | **Parameter Type** | **Mandatory** | **Desc**       |
| ------------------ | ------------------ | ------------- | -------------- |
| symbol             | string             | true          | "BTC","ETH"... |

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                      | **Value Range** |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | --------------- |
| status                         | true          | string   | Request Processing Result                     | "ok" , "error"  |
| \<list\>(Attribute Name: data) |               |          |                                               |                 |
| symbol                         | true          | string   | symbol                                        | "BTC","ETH"...  |
| index_price                    | true          | decimal  | Index Price                                   |                 |
| \</list\>                      |               |          |                                               |                 |
| ts                             | true          | long     | Time of Respond Generation，Unit：Millisecond |                 |

**Example**
```
GET  https://api.hbdm.com/api/v1/contract_index?symbol=BTC

# Response
{
  "status":"ok",
  "data": [
     {
       "symbol": "BTC",
       "index_price":471.0817
      }
    ],
  "ts": 1490759594752
}
```
#### <a name="3">Contract Price Limitation</a>

URL /v1/contract_price_limit

**Request Parameter**

| **Parameter Name** | **Parameter Type** | **Mandatory** | **Desc**                                          |
| ------------------ | ------------------ | ------------- | ------------------------------------------------- |
| symbol             | string             | false         | "BTC","ETH"...                                    |
| contract_type      | string             | false         | Contract Type ("this_week","next_week","quarter") |
| contract_code      | string             | false         | BTC180914  ...                                    |

**Note**：If there is a number in the Contract Code row，inquiry with Contract_Code. If there is no number，inquiry by Symbol + Contract Type. One of the query conditions must be chosen

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                      | **Value Range**                   |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | --------------------------------- |
| status                         | true          | string   | Request Processing Result                     | "ok" ,"error"                     |
| \<list\>(Attribute Name: data) |               |          |                                               |                                   |
| symbol                         | true          | string   | Variety code                                  | "BTC","ETH" ...                   |
| high_limit                     | true          | decimal  | Highest Buying Price                          |                                   |
| low_limit                      | true          | decimal  | Lowest Selling Price                          |                                   |
| contract_code                  | true          | string   | Contract Code                                 | eg "BTC180914"  ...               |
| contract_type                  | true          | string   | Contract Type                                 | "this_week","next_week","quarter" |
| \<list\>                       |               |          |                                               |                                   |
| ts                             | true          | long     | Time of Respond Generation, Unit: Millisecond |                                   |

**Example**
```
GET  https://api.hbdm.com/api/v1/contract_price_limit?symbol=BTC&contract_type=this_week

# Response
{
  "status":"ok",
  "data": 
    {
      "symbol":"BTC",
      "high_limit":443.07,
      "low_limit":417.09,
      "contract_code":"BTC180914",
      "contract_type":"this_week"
     },
  "ts": 1490759594752
}
```
#### <a name="4">Get Contract Open Interest Information </a>

URL /v1/contract_open_interest

**Request Parameter**

| **Parameter Name** | **Parameter Type** | **Mandatory** | **Desc**                                          |
| ------------------ | ------------------ | ------------- | ------------------------------------------------- |
| symbol             | string             | false         | "BTC","ETH"...                                    |
| contract_type      | string             | false         | Contract Type ("this_week","next_week","quarter") |
| contract_code      | string             | false         | BTC180914                                         |

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                      | **Value Range**                   |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | --------------------------------- |
| status                         | true          | string   | Request Processing Result                     | "ok" , "error"                    |
| \<list\>(Attribute Name: data) |               |          |                                               |                                   |
| symbol                         | true          | string   | Variety code                                  | "BTC", "ETH" ...                  |
| contract_type                  | true          | string   | Contract Type                                 | "this_week","next_week","quarter" |
| volume                         | true          | decimal  | Position quantity(amount)                     |                                   |
| amount                         | true          | decimal  | Position quantity(Currency)                   |                                   |
| contract_code                  | true          | string   | Contract Code                                 | eg "BTC180914"   ...              |
| \</list\>                      |               |          |                                               |                                   |
| ts                             | true          | long     | Time of Respond Generation, Unit: Millisecond |                                   |

**Example**
```
GET  https://api.hbdm.com/api/v1/contract_open_interest?symbol=BTC&contract_type=this_week

# Response
{
  "status":"ok",
  "data":
    {
      "symbol":"BTC",
      "contract_type": "this_week",
      "volume":123,
      "amount":106,
      "contract_code": "BTC180914"
     },
  "ts": 1490759594752
}
```
#### <a name="5">Get Market Depth</a>

URL /market/depth

**Request Parameter**

| **Parameter Name** | **Parameter Type** | **Mandatory** | **Desc**                                                     |
| ------------------ | ------------------ | ------------- | ------------------------------------------------------------ |
| symbol             | string             | true          | e.g. "BTC_CQ" represents BTC “This Week”，"BTC_CQ" represents BTC “Next Week”，"BTC_CQ" represents BTC “Quarter” |
| type               | string             | true          | step0, step1, step2, step3, step4, step5（merged deep data 0-5）；when step is 0，deep data not merged |

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Data Type** | **Desc**                                                     | **Value Range** |
| ------------------ | ------------- | ------------- | ------------------------------------------------------------ | --------------- |
| ch                 | true          | string        | Data belonged channel，Format： market.period                |                 |
| status             | true          | string        | Request Processing Result                                    | "ok" , "error"  |
| asks               | true          | object        | Selling, [price(hanging unit Price), vol(this price represent single contract)], According to the ascending order of Price |                 |
| bids               | true          | object        | Buying, [price(hanging unit price), vol(this price represent single contract)], According to the descending order of Price |                 |
| ts                 | true          | number        | Time of Respond Generation，Unit：Millisecond                |                 |
```
tick illustration:

"tick": {
    "id": Message id.
    "ts": Time of Message Generation, unit: millisecond
    "bids": Buying, [price(hanging unit price), vol(this price represent single contract)], According to the descending order of Price
    "asks": Selling, [price(hanging unit Price), vol(this price represent single contract)], According to the ascending order of Price  
    }

```
**Example**
```
GET https://api.hbdm.com/market/depth?symbol=BTC_CQ&type=step5

# Response
{
  "ch":"market.BTC_CQ.depth.step5",
  "status":"ok",
    "tick":{
      "asks":[
        [6580,3000],
        [70000,100]
        ],
      "bids":[
        [10,3],
        [2,1]
        ],
      "ch":"market.BTC_CQ.depth.step5",
      "id":1536980854,
      "mrid":6903717,
      "ts":1536980854171,
      "version":1536980854
    },
  "ts":1536980854585
}
```
#### <a name="6">Get K-Line Data</a>

URL
/market/history/kline

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**             | **Default** | **Value Range**                                              |
| ------------------ | ------------- | -------- | -------------------- | ----------- | ------------------------------------------------------------ |
| symbol             | true          | string   | Contract Name        |             | e.g. "BTC_CQ" represents BTC “This Week”，"BTC_CQ" represents BTC “Next Week”，"BTC_CQ" represents BTC “Quarter” |
| period             | true          | string   | K-Line Type          |             | 1min, 5min, 15min, 30min, 60min, 1hour,4hour,1day, 1mon      |
| size               | false         | integer  | Acquisition Quantity | 150         | [1,2000]                                                     |

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Data Type** | **Desc**                                      | **Value Range** |
| ------------------ | ------------- | ------------- | --------------------------------------------- | --------------- |
| ch                 | true          | string        | Data belonged channel，Format： market.period |                 |
| data               | true          | object        | KLine Data                                    |                 |
| status             | true          | string        | Request Processing Result                     | "ok" , "error"  |
| ts                 | true          | number        | Time of Respond Generation, Unit: Millisecond |                 |

Data Illustration：
```
"data": [
  {
        "id": K-Line id,
        "vol": Transaction Volume(amount),
        "count": transaction count
        "open": opening Price
        "close": Closing Price, when the K-line is the latest one，it means the latest price
        "low": Lowest price
        "high": highest price
        "amount": transaction volume(currency), sum(every transaction volume(amount)*every contract value/transaction price for this contract)
   }
]
```
**Example**
```
GET  https://api.hbdm.com/market/history/kline?period=1min&size=200&symbol=BTC_CQ

# Response
{
  "ch": "market.BTC_CQ.kline.1min",
  "data": [
    {
      "vol": 2446,
      "close": 5000,
      "count": 2446,
      "high": 5000,
      "id": 1529898120,
      "low": 5000,
      "open": 5000,
      "amount": 48.92
     },
    {
      "vol": 0,
      "close": 5000,
      "count": 0,
      "high": 5000,
      "id": 1529898780,
      "low": 5000,
      "open": 5000,
      "amount": 0
     }
   ],
  "status": "ok",
  "ts": 1529908345313
}


```
####  <a name="7">Get Market Data Overview</a>

URL
/market/detail/merged

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**      | **Default** | **Value Range**                                              |
| ------------------ | ------------- | -------- | ------------- | ----------- | ------------------------------------------------------------ |
| symbol             | true          | string   | Contract Name |             | e.g. "BTC_CQ" represents BTC “This Week”，"BTC_CQ" represents BTC “Next Week”，"BTC_CQ" represents BTC “Quarter” |

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Data Type** | **Desc**                                                     | **Value Range** |
| ------------------ | ------------- | ------------- | ------------------------------------------------------------ | --------------- |
| ch                 | true          | string        | Data belonged channel，format： market.$symbol.detail.merged |                 |
| status             | true          | string        | Request Processing Result                                    | "ok" , "error"  |
| tick               | true          | object        | K-Line Data                                                  |                 |
| ts                 | true          | number        | Time of Respond Generation, Unit: Millisecond                |                 |

tick Illustration
```
"tick": {
    "id": K-Line id,
    "vol": transaction volume（contract）,
    "count": transaction count
    "open": opening price,
    "close": Closing Price, when the K-line is the latest one，it means the latest price
        "low": Lowest price
        "high": highest price
        "amount": transaction volume(currency), sum(every transaction volume(amount)*every contract value/transaction price for this contract)
    "bid": [price of buying one (amount)],
    "ask": [price of selling one (amount)]

  }
```
**Example**
```
GET  https://api.hbdm.com/market/detail/merged?symbol=BTC_CQ

# Response
{
  "ch": "market.BTC_CQ.detail.merged",
  "status": "ok",
  "tick": 
    {
      "vol":"13305",
      "ask": [5001, 2],
      "bid": [5000, 1],
      "close": "5000",
      "count": "13305",
      "high": "5000",
      "id": 1529387841,
      "low": "5000",
      "open": "5000",
      "ts": 1529387842137,
      "amount": "266.1"
     },
  "ts": 1529387842137
}
```
#### <a name="8">The Last Trade of a Contract</a>

URL /market/trade

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**      | **Default** | **Value Range**                                              |
| ------------------ | ------------- | -------- | ------------- | ----------- | ------------------------------------------------------------ |
| symbol             | true          | string   | Contract Name |             | e.g. "BTC_CQ" represents BTC “This Week”，"BTC_CQ" represents BTC “Next Week”，"BTC_CQ" represents BTC “Quarter” |

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                                                    | **Default** | **Value Range** |
| ------------------ | ------------- | -------- | ----------------------------------------------------------- | ----------- | --------------- |
| ch                 | true          | string   | Data belonged channel，Format： market.$symbol.trade.detail |             |                 |
| status             | true          | string   |                                                             |             | "ok","error"    |
| tick               | true          | object   | Trade Data                                                  |             |                 |
| ts                 | true          | number   | Sending time                                                |             |                 |

Tick Illustration：
```
"tick": {
  "id": Message id,
  "ts": Latest Transaction time,
  "data": [
    {
      "id": Transaction id,
      "price": Transaction price,
      "amount": transaction amount,
      "direction": Active transaction direction
      "ts": transaction time

    }
  ]
}
```


**Example**
```
GET  https://api.hbdm.com/market/trade?symbol=BTC_CQ

# Response
{
  "ch": "market.BTC_CQ.trade.detail",
  "status": "ok",
  "tick": {
    "data": [
      {
        "amount": "1",
        "direction": "sell",
        "id": 6010881529486944176,
        "price": "5000",
        "ts": 1529386945343
       }
     ],
    "id": 1529388202797,
    "ts": 1529388202797
    },
  "ts": 1529388202797
}
```
#### <a name="9">Request a Batch of Trade Records of a Contract</a>

URL /market/history/trade

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Data Type** | **Desc**                              | **Default** | **Value Range**                                              |
| ------------------ | ------------- | ------------- | ------------------------------------- | ----------- | ------------------------------------------------------------ |
| symbol             | true          | string        | Contract Name                         |             | e.g. "BTC_CQ" represents BTC “This Week”，"BTC_CQ" represents BTC “Next Week”，"BTC_CQ" represents BTC “Quarter” |
| size               | false         | number        | Number of Trading Records Acquisition | 1           | [1, 2000]                                                    |

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Data Type** | **Desc**                                                    | **Value Range** |
| ------------------ | ------------- | ------------- | ----------------------------------------------------------- | --------------- |
| ch                 | true          | string        | Data belonged channel，Format： market.$symbol.trade.detail |                 |
| data               | true          | object        | Trade Data                                                  |                 |
| status             | true          | string        |                                                             | "ok"，"error"   |
| ts                 | true          | number        | Time of Respond Generation, Unit: Millisecond               |                 |

data Illustration：
```
"data": {
  "id": Message id,
  "ts": Latest transaction time,
  "data": [
    {
      "id": Transaction id,
      "price": transaction price,
      "amount": transaction (amount),
      "direction": active transaction direction
      "ts": transaction time
      }
}
```


**Example**
```
GET  https://api.hbdm.com/market/history/trade?symbol=BTC_CQ&size=100

# Response
{
  "ch": "market.BTC_CQ.trade.detail",
  "status": "ok",
  "ts": 1529388050915,
  "data": [
    {
      "id": 601088,
      "ts": 1529386945343,
      "data": [
        {
         "amount": 1,
         "direction": "sell",
         "id": 6010881529486944176,
         "price": 5000,
         "ts": 1529386945343
         }
       ]
    }
   ]
}

```

Account Interface
--------

#### <a name="101">User’s Account Information</a>

URL  /v1/contract_account_info 

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**     | **Default** | **Value Range**                                         |
| ------------------ | ------------- | -------- | ------------ | ----------- | ------------------------------------------------------- |
| symbol             | false         | string   | Variety code |             | "BTC","ETH"...if default, return to all types defaulted |

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                      | **Value Range** |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | --------------- |
| status                         | true          | string   | Request Processing Result                     | "ok" , "error"  |
| \<list\>(Attribute Name: data) |               |          |                                               |                 |
| symbol                         | true          | string   | Variety code                                  | "BTC","ETH"...  |
| margin_balance                 | true          | decimal  | Account rights                                |                 |
| margin_position                | true          | decimal  | Position Margin                               |                 |
| margin_frozen                  | true          | decimal  | Freeze margin                                 |                 |
| margin_available               | true          | decimal  | Available margin                              |                 |
| profit_real                    | true          | decimal  | Realized profit                               |                 |
| profit_unreal                  | true          | decimal  | Unrealized profit                             |                 |
| risk_rate                      | true          | decimal  | risk rate                                     |                 |
| liquidation_price              | true          | decimal  | Estimated liquidation price                   |                 |
| withdraw_available             | true          | decimal  | Available withdrawal                          |                 |
| lever_rate                     | true          | decimal  | Leverage Rate                                 |                 |
| \</list\>                      |               |          |                                               |                 |
| ts                             | number        | long     | Time of Respond Generation, Unit: Millisecond |                 |

**Example**
```
POST  https://api.hbdm.com/api/v1/contract_account_info    

# Response
{
  "status": "ok",
  "data": [
    {
      "symbol": "BTC",
      "margin_balance": 1,
      "margin_position": 0,
      "margin_frozen": 3.33,
      "margin_available": 0.34,
      "profit_real": 3.45,
      "profit_unreal": 7.45,
      "withdraw_available":4.0989898,
      "risk_rate": 100,
      "liquidation_price": 100
     },
    {
      "symbol": "ETH",
      "margin_balance": 1,
      "margin_position": 0,
      "margin_frozen": 3.33,
      "margin_available": 0.34,
      "profit_real": 3.45,
      "profit_unreal": 7.45,
      "withdraw_available":4.7389859,
      "risk_rate": 100,
      "liquidation_price": 100
     }
   ],
  "ts":158797866555
}

```
#### <a name="102">User’s position Information </a>

URL /v1/contract_position_info

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**     | **Default** | **Value Range**                                         |
| ------------------ | ------------- | -------- | ------------ | ----------- | ------------------------------------------------------- |
| symbol             | false         | string   | Variety code |             | "BTC","ETH"...if default, return to all types defaulted |

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                      | **Value Range**                     |
| ------------------------------ | ------------- | -------- | --------------------------------------------- | ----------------------------------- |
| status                         | true          | string   | Request Processing Result                     | "ok" , "error"                      |
| \<list\>(Attribute Name: data) |               |          |                                               |                                     |
| symbol                         | true          | string   | Variety code                                  | "BTC","ETH"...                      |
| contract_code                  | true          | string   | Contract Code                                 | "BTC180914" ...                     |
| contract_type                  | true          | string   | Contract Type                                 | "this_week", "next_week", "quarter" |
| volume                         | true          | decimal  | Position quantity                             |                                     |
| available                      | true          | decimal  | Available position can be closed              |                                     |
| frozen                         | true          | decimal  | frozen                                        |                                     |
| cost_open                      | true          | decimal  | Opening average price                         |                                     |
| cost_hold                      | true          | decimal  | Average price of position                     |                                     |
| profit_unreal                  | true          | decimal  | Unrealized profit and loss                    |                                     |
| profit_rate                    | true          | decimal  | Profit rate                                   |                                     |
| profit                         | true          | decimal  | profit                                        |                                     |
| position_margin                | true          | decimal  | Position margin                               |                                     |
| lever_rate                     | true          | int      | Leverage rate                                 |                                     |
| direction                      | true          | string   | Transaction direction                         |                                     |
| \</list\>                      |               |          |                                               |                                     |
| ts                             | true          | long     | Time of Respond Generation, Unit: Millisecond |                                     |

**Example**
```
POST  https://api.hbdm.com/api/v1/contract_position_info

# Response
{
  "status": "ok",
  "data": [
    {
      "symbol": "BTC",
      "contract_code": "BTC180914",
      "contract_type": "this_week",
      "volume": 1,
      "available": 0,
      "frozen": 0.3,
      "cost_open": 422.78,
      "cost_hold": 422.78,
      "profit_unreal": 0.00007096,
      "profit_rate": 0.07,
      "profit": 0.97,
      "position_margin": 3.4,
      "lever_rate": 10,
      "direction":"buy"
     }
    ],
 "ts": 158797866555
}
```


Trade Interface
--------

#### <a name="103"> Place an Order </a>

URL /v1/contract_order

**Request Parameter**

| **Parameter Name** | **Parameter Type** | **Mandatory** | **Desc**                                                     |
| ------------------ | ------------------ | ------------- | ------------------------------------------------------------ |
| symbol             | string             | false         | "BTC","ETH"...                                               |
| contract_type      | string             | false         | Contract Type ("this_week": "next_week": "quarter":)         |
| contract_code      | string             | false         | BTC180914                                                    |
| client_order_id    | long               | false         | Clients fill and maintain themselves, and this time must be greater than last time |
| price              | decimal            | true          | Price                                                        |
| volume             | long               | true          | Numbers of orders (amount)                                   |
| direction          | string             | true          | Transaction direction                                        |
| offset             | string             | true          | "open", "close"                                              |
| lever_rate         | int                | true          | Leverage rate [if“Open”is multiple orders in 10 rate, there will be not multiple orders in 20 rate |
| order_price_type   | string             | true          | "limit", "opponent"                                          |

**Note**： If there is a number in the Contract Code row，inquiry with Contract_Code. If there is no number，inquiry by Symbol + Contract Type

**Returning Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                                                     | **Value Range** |
| ------------------ | ------------- | -------- | ------------------------------------------------------------ | --------------- |
| status             | true          | string   | Request Processing Result                                    | "ok" , "error"  |
| order_id           | true          | long     | Order ID                                                     |                 |
| client_order_id    | true          | long     | the client ID that is filled in when the order is placed, if it’s not filled, it won’t be returned |                 |
| ts                 | true          | long     | Time of Respond Generation, Unit: Millisecond                |                 |

**Example**
```
POST  https://api.hbdm.com/api/v1/contract_order

# Response
{
  "status": "ok",
  "order_id": 986,
  "client_order_id": 9086,
  "ts": 158797866555
}
```
#### <a name="104"> Place a Batch of Orders</a>

URL /v1/contract_batchorder

**Request Parameter**

| **Parameter Name**                    | **Parameter Type** | **Mandatory** | **Desc**                                                     |
| ------------------------------------- | ------------------ | ------------- | ------------------------------------------------------------ |
| \<list\>(Attribute Name: orders_data) |                    |               |                                                              |
| symbol                                | string             | false         | "BTC","ETH"...                                               |
| contract_type                         | string             | false         | Contract Type: "this_week": "next_week": "quarter":          |
| contract_code                         | string             | false         | BTC180914                                                    |
| client_order_id                       | long               | true          | Clients fill and maintain themselves, and this time must be greater than last time |
| price                                 | decimal            | true          | Price                                                        |
| volume                                | long               | true          | Numbers of orders (amount)                                   |
| direction                             | string             | true          | Transaction direction                                        |
| offset                                | string             | true          | "open": "close"                                              |
| lever_rate                            | int                | true          | Leverage rate [if“Open”is multiple orders in 10 rate, there will be not multiple orders in 20 rate |
| order_price_type                      | string             | true          | "limit":   "opponent"                                        |
| \</list\>                             |                    |               |                                                              |

**Note**：If there is a number in the Contract Code row，inquiry with Contract_Code. If there is no number，inquiry by Symbol + Contract Type

**Returning Parameter**

| **Parameter Name**                | **Mandatory** | **Type** | **Desc**                                                     | **Value Range** |
| --------------------------------- | ------------- | -------- | ------------------------------------------------------------ | --------------- |
| status                            | true          | string   | Request Processing Result                                    | "ok" , "error"  |
| \<list\>(Attribute Name: errors)  |               |          |                                                              |                 |
| index                             | true          | int      | order Index                                                  |                 |
| err_code                          | true          | int      | Error code                                                   |                 |
| err_msg                           | true          | string   | Error information                                            |                 |
| \</list\>                         |               |          |                                                              |                 |
| \<list\>(Attribute Name: success) |               |          |                                                              |                 |
| index                             | true          | int      | order Index                                                  |                 |
| order_id                          | true          | long     | Order ID                                                     |                 |
| client_order_id                   | true          | long     | the client ID that is filled in when the order is placed, if it’s not filled, it won’t be returned |                 |
| \</list\>                         |               |          |                                                              |                 |
| ts                                | true          | long     | Time of Respond Generation, Unit: Millisecond                |                 |

**Example**
```
POST  https://api.hbdm.com/api/v1/contract_batchorder

# Response
{
  "status": "ok",
  "data": {
    "errors":[
      {
        "index":0,
        "err_code": 200417,
        "err_msg": "invalid symbol"
       },
      {
        "index":3,
        "err_code": 200415,
        "err_msg": "invalid symbol"
       }
     ],
    "success":[
      {
        "index":1,
        "order_id":161256,
        "client_order_id":1344567
       },
      {
        "index":2,
        "order_id":161257,
        "client_order_id":1344569
       }
     ]
   },
  "ts": 1490759594752
}
```
#### <a name="105">Cancel an Order </a>

URL /v1/contract_cancel

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                                                     |
| ------------------ | ------------- | -------- | ------------------------------------------------------------ |
| order_id           | false         | string   | Order ID（different IDs are separated by ",", maximum 50 orders can be withdrew at one time） |
| client_order_id    | false         | string   | Client order ID (different IDs are separated by ",", maximum 50 orders can be withdrew at one time) |
| symbol             | true          | string   | "BTC","ETH"...                                               |

**Note**：
Both order_id and client_order_id can be used for order withdrawl，one of them needed at one time，if both of them are set，the default will be order id。

**Returning Parameter**

| **Parameter Name**               | **Mandatory** | **Type** | **Desc**                                                  | **Value Range** |
| -------------------------------- | ------------- | -------- | --------------------------------------------------------- | --------------- |
| status                           | true          | string   | Request Processing Result                                 | "ok" , "error"  |
| \<list\>(Attribute Name: errors) |               |          |                                                           |                 |
| order_id                         | true          | string   | Order ID                                                  |                 |
| err_code                         | true          | int      | Error code                                                |                 |
| err_msg                          | true          | string   | Error information                                         |                 |
| \</list\>                        |               |          |                                                           |                 |
| successes                        | true          | string   | Successfully withdrew list of order_id or client_order_id |                 |
| ts                               | true          | long     | Time of Respond Generation, Unit: Millisecond             |                 |

**Example**
```
POST  https://api.hbdm.com.com/api/v1/contract_cancel

# Response
# result of multiple order withdrawls (successful withdrew order ID, failed withdrew order ID)
{
  "status": "ok",
  "errors":[
    {
      "order_id":161251,
      "err_code": "1002",
      "err_msg": "order doesn’t exist"
     },
    {
      "order_id":161253,
      "err_code": "1002",
      "err_msg": "order doesn’t exist"
     }
   ],
  "success":["161256","1344567"],
  "ts": 1490759594752
}
```
#### <a name="106">Cancel All Orders </a>

URL /v1/contract_cancelall

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                        |
| ------------------ | ------------- | -------- | ------------------------------- |
| symbol             | true          | string   | Variety code，eg "BTC","ETH"... |

**Returning Parameter**

| **Parameter Name**               | **Mandatory** | **Type** | **Desc**                                      | **Value Range** |
| -------------------------------- | ------------- | -------- | --------------------------------------------- | --------------- |
| status                           | true          | string   | Request Processing Result                     | "ok" , "error"  |
| successes                        | true          | string   | Successful order                              |                 |
| \<list\>(Attribute Name: errors) |               |          |                                               |                 |
| order_id                         | true          | String   | Order ID                                      |                 |
| err_code                         | true          | int      | failed order error messageError code          |                 |
| err_msg                          | true          | int      | failed order information                      |                 |
| \</list\>                        |               |          |                                               |                 |
| successes                        | true          | string   | Successful order                              |                 |
| ts                               | true          | long     | Time of Respond Generation, Unit: Millisecond |                 |

**Example**
```
POST  https://api.hbdm.com.com/api/v1/contract_cancel      

# Response
{
 "symbol": "BTC"
}
# Response
# result of multiple order withdrawls (successful withdrew order ID, failed withdrew order ID)
{
  "status": "ok",
  "data": {
    "errors":[
      {
        "order_id":"161251",
        "err_code": 200417,
        "err_msg": "invalid symbol"
       },
      {
        "order_id":161253,
        "err_code": 200415,
        "err_msg": "invalid symbol"
       }
      ],
    "successes":[161256,1344567]
   },
  "ts": 1490759594752
}
Error：
{
  "status": "error",
  "err_code": 20012,
  "err_msg": "invalid symbol",
  "ts": 1490759594752
}
```
#### <a name="107">Get Information of an Order </a>

URL /v1/contract_order_info

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                                                     |
| ------------------ | ------------- | -------- | ------------------------------------------------------------ |
| order_id           | false         | string   | Order ID（different IDs are separated by ",", maximum 20 orders can be withdrew at one time） |
| client_order_id    | false         | string   | Client order ID Order ID（different IDs are separated by ",", maximum 20 orders can be withdrew at one time) |
| symbol             | true          | string   | "BTC","ETH"...                                               |

**Note**：Both order_id and client_order_id can be used for order withdrawl，one of them needed at one time，if both of them are set，the default will be order id。

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                                     | **Value Range**                     |
| ------------------------------ | ------------- | -------- | ------------------------------------------------------------ | ----------------------------------- |
| status                         | true          | string   | Request Processing Result                                    | "ok" , "error"                      |
| \<list\>(Attribute Name: data) |               |          |                                                              |                                     |
| symbol                         | true          | string   | Variety code                                                 |                                     |
| contract_type                  | true          | string   | Contract Type                                                | "this_week", "next_week", "quarter" |
| contract_code                  | true          | string   | Contract Code                                                | "BTC180914" ...                     |
| volume                         | true          | decimal  | Numbers of order                                             |                                     |
| price                          | true          | decimal  | Price committed                                              |                                     |
| order_price_type               | true          | string   | Order price type [limited price，opponent price，market price] |                                     |
| direction                      | true          | string   | Transaction direction                                        |                                     |
| offset                         | true          | string   | "open": "close"                                              |                                     |
| lever_rate                     | true          | int      | Leverage rate                                                | 1\\5\\10\\20                        |
| order_id                       | true          | long     | Order ID                                                     |                                     |
| client_order_id                | true          | long     | Client order ID                                              |                                     |
| created_at                     | true          | long     | Transaction time                                             |                                     |
| trade_volume                   | true          | decimal  | Transaction quantity                                         |                                     |
| trade_turnover                 | true          | decimal  | Transaction aggregate amount                                 |                                     |
| fee                            | true          | decimal  | Servicefee                                                   |                                     |
| trade_avg_price                | true          | decimal  | Transaction average price                                    |                                     |
| margin_frozen                  | true          | decimal  | Freeze margin                                                |                                     |
| profit                         | true          | decimal  | profit                                                       |                                     |
| status                         | true          | int      | Order status (1ready to submit 2ordered 3submitted 4partially transacted 5partially withdrew 6all transacted 7withdrew 11withdrawing) |                                     |
| order_source                   | true          | string   | Order source（1:system order、2:web page order、3:API order、4:APP order 5liquidation source、6delivery source） |                                     |
| \</list\>                      |               |          |                                                              |                                     |
| ts                             | true          | long     | Timestamp                                                    |                                     |

**Example**
```
POST  https://api.hbdm.com.com/api/v1/contract_order_info

# Response
{
  "status": "ok",
  "data":[
    {
      "symbol": "BTC",
      "contract_type": "this_week",
      "contract_code": "BTC180914",
      "volume": 111,
      "price": 1111,
      "order_price_type": "limit",
      "direction": "buy",
      "offset": "open",
      "lever_rate": 10,
      "order_id": 106837,
      "client_order_id": 10683,
      "order_source": "web",
      "created_at": 1408076414000,
      "trade_volume": 1,
      "trade_turnover": 1200,
      "fee": 0,
      "trade_avg_price": 10,
      "margin_frozen": 10,
      "profit ": 10,
      "status": 0
     },
    {
      "symbol": "ETH",
      "contract_type": "this_week",
      "contract_code": "ETH180921",
      "volume": 111,
      "price": 1111,
      "order_price_type": "limit",
      "direction": "buy",
      "offset": "open",
      "lever_rate": 10,
      "order_id": 106837,
      "client_order_id": 10683,
      "order_source": "web",
      "created_at": 1408076414000,
      "trade_volume": 1,
      "trade_turnover": 1200,
      "fee": 0,
      "trade_avg_price": 10,
      "margin_frozen": 10,
      "profit ": 10,
      "status": 0
     }
    ],
  "ts": 1490759594752
}
```
#### <a name="108">Order details acquisition </a>

URL /v1/contract_order_detail

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                      |
| ------------------ | ------------- | -------- | ----------------------------- |
| symbol             | true          | string   | "BTC","ETH"...                |
| order_id           | true          | long     | Order ID                      |
| createAt           | true          | long     | Timestamp                     |
| page_index         | false         | int      | Page number, default 1st page |
| page_size          | false         | int      | Default 20，no more than 50   |

**返回数据**

| **Parameter Name**                | **Mandatory** | **Type** | **Desc**                                                     | **Value Range**                   |
| --------------------------------- | ------------- | -------- | ------------------------------------------------------------ | --------------------------------- |
| status                            | true          | string   | Request Processing Result                                    | "ok" , "error"                    |
| \<object\> (Attribute Name: data) |               |          |                                                              |                                   |
| symbol                            | true          | string   | Variety code                                                 |                                   |
| contract_type                     | true          | string   | Contract Type                                                | "this_week","next_week","quarter" |
| contract_code                     | true          | string   | Contract Code                                                | "BTC180914" ...                   |
| lever_rate                        | true          | int      | Leverage Rate                                                | 1\\5\\10\\20                      |
| direction                         | true          | string   | Transaction direction                                        |                                   |
| offset                            | true          | string   | "open": "close"                                              |                                   |
| volume                            | true          | decimal  | Number of Order                                              |                                   |
| price                             | true          | decimal  | Price committed                                              |                                   |
| created_at                        | true          | long     | Transaction time                                             |                                   |
| order_source                      | true          | string   | Order Source                                                 |                                   |
| order_price_type                  | true          | string   | Order price type [limited price，opponent price，market price] |                                   |
| margin_frozen                     | true          | decimal  | Freeze margin                                                |                                   |
| profit                            | true          | decimal  | profit                                                       |                                   |
| total_page                        | true          | int      | Page in total                                                |                                   |
| current_page                      | true          | int      | Current Page                                                 |                                   |
| total_size                        | true          | int      | Total Size                                                   |                                   |
| \<list\> (Attribute Name: trades) |               |          |                                                              |                                   |
| trade_id                          | true          | long     | Match Result id                                              |                                   |
| trade_price                       | true          | decimal  | Match Price                                                  |                                   |
| trade_volume                      | true          | decimal  | Transaction quantity                                         |                                   |
| trade_turnover                    | true          | decimal  | Transaction price                                            |                                   |
| trade_fee                         | true          | decimal  | Transaction Service fee                                      |                                   |
| created_at                        | true          | long     | Creation time                                                |                                   |
| \</list\>                         |               |          |                                                              |                                   |
| \</object \>                      |               |          |                                                              |                                   |
| ts                                | true          | long     | Timestamp                                                    |                                   |

**Example**
```
POST  https://api.hbdm.com/api/v1/contract_order_detail

# Response
{
  "status": "ok",
  "data":{
    "symbol": "BTC",
    "contract_type": "this_week",
    "contract_code": "BTC180914",
    "volume": 111,
    "price": 1111,
    "order_price_type": "limit",
    "direction": "buy",
    "offset": "open",
    "status": 1,
    "lever_rate": 10,
    "trade_avg_price": 10,
    "margin_frozen": 10,
    "profit": 10,
    "order_id": 106837,
    "order_source": "web",
    "created_at": 1408076414000,
    "trades":[
      {
        "trade_id":112,
        "trade_volume":1,
        "trade_price":123.4555,
        "trade_fee":0.234,
        "trade_turnover":34.123,
        "created_at": 1490759594752
       }
      ],
    "total_page":15,
    "total_size":3,
    "current_page":3
    },
  "ts": 1490759594752
}
Error:
{
 "status":"error",
 "err_code":20029,
 "err_msg": "invalid symbol",
 "ts": 1490759594752
}
```
#### <a name="109">Current unfilled commission acquisition </a>

URL  /v1/contract_openorders

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                    | **Default** | **Value Range** |
| ------------------ | ------------- | -------- | --------------------------- | ----------- | --------------- |
| symbol             | false         | string   | Variety code                |             | "BTC","ETH"...  |
| page_index         | false         | int      | Page, default 1st page      | 1           |                 |
| page_size          | false         | int      | Default 20，no more than 50 | 20          |                 |

**Returning Parameter**

| **Parameter Name**             | **Mandatory** | **Type** | **Desc**                                                     | **Value Range**                   |
| ------------------------------ | ------------- | -------- | ------------------------------------------------------------ | --------------------------------- |
| status                         | true          | string   | Request Processing Result                                    |                                   |
| \<list\>(Attribute Name: data) |               |          |                                                              |                                   |
| symbol                         | true          | string   | Variety code                                                 |                                   |
| contract_type                  | true          | string   | Contract Type                                                | "this_week","next_week","quarter" |
| contract_code                  | true          | string   | Contract Code                                                | "BTC180914" ...                   |
| volume                         | true          | decimal  | Number of Order                                              |                                   |
| price                          | true          | decimal  | Price committed                                              |                                   |
| order_price_type               | true          | string   | Order price type [limited price，opponent price，market price] |                                   |
| direction                      | true          | string   | Transaction direction                                        |                                   |
| offset                         | true          | string   | "open": "close"                                              |                                   |
| lever_rate                     | true          | int      | Leverage Rate                                                | 1\\5\\10\\20                      |
| order_id                       | true          | long     | Order ID                                                     |                                   |
| client_order_id                | true          | long     | Client order ID                                              |                                   |
| created_at                     | true          | long     | Order Creation time                                          |                                   |
| trade_volume                   | true          | decimal  | Transaction quantity                                         |                                   |
| trade_turnover                 | true          | decimal  | Transaction aggregate amount                                 |                                   |
| fee                            | true          | decimal  | Servicefee                                                   |                                   |
| trade_avg_price                | true          | decimal  | Transaction average price                                    |                                   |
| margin_frozen                  | true          | decimal  | Freeze margin                                                |                                   |
| profit                         | true          | decimal  | profit                                                       |                                   |
| status                         | true          | int      | Order status (3didn’t transact 4partially transacted 5partially withdrew 6all transacted 7withdrew) |                                   |
| order_source                   | true          | string   | Order Source                                                 |                                   |
| \</list\>                      |               |          |                                                              |                                   |
| total_page                     | true          | int      | Total Pages                                                  |                                   |
| current_page                   | true          | int      | Current Page                                                 |                                   |
| total_size                     | true          | int      | Total Size                                                   |                                   |
| ts                             | true          | long     | Timestamp                                                    |                                   |

**Example**
```
POST https://www.hbdm.com/api/v1/contract_openorders

# Response
{
  "status": "ok",
  "data":{
    "orders":[
      {
         "symbol": "BTC",
         "contract_type": "this_week",
         "contract_code": "BTC180914",
         "volume": 111,
         "price": 1111,
         "order_price_type": "limit",
         "direction": "buy",
         "offset": "open",
         "lever_rate": 10,
         "order_id": 106837,
         "client_order_id": 10683,
         "order_source": "web",
         "created_at": 1408076414000,
         "trade_volume": 1,
         "trade_turnover": 1200,
         "fee": 0,
         "trade_avg_price": 10,
         "margin_frozen": 10,
         "status": 1
        }
       ],
    "total_page":15,
    "current_page":3,
    "total_size":3
   },
  "ts": 1490759594752
}
```
#### <a name="110">Get History Orders</a>

URL /v1/contract_hisorders

**Request Parameter**

| **Parameter Name** | **Mandatory** | **Type** | **Desc**                    | **Default** | **Value Range**                                              |
| ------------------ | ------------- | -------- | --------------------------- | ----------- | ------------------------------------------------------------ |
| symbol             | true          | string   | Variety code                |             | "BTC","ETH"...                                               |
| trade_type         | true          | int      | Transaction type            |             | 0:all,1: buy long,2: sell short,3: buy short,4: sell  long,5: sell liquidation,6: buy liquidation,7:Delivery long,8: Delivery short |
| type               | true          | int      | Type                        |             | 1:All Orders,2:Order in Finished Status                      |
| status             | true          | int      | Order Status                |             | 0:all 3:unsettled, 4: partly transacted,5: partly transaction withdrawl,6: all transacted,7:withdrew |
| create_date        | true          | int      | Date                        |             | 7，90（7days or 90 days）                                    |
| page_index         | false         | int      | Page, default 1st page      | 1           |                                                              |
| page_size          | false         | int      | Default 20，no more than 50 | 20          |                                                              |

**Returning Parameter**

| **Parameter Name**               | **Mandatory** | **Type** | **Desc**                                                     | **Value Range**                   |
| -------------------------------- | ------------- | -------- | ------------------------------------------------------------ | --------------------------------- |
| status                           | true          | string   | Request Processing Result                                    |                                   |
| \<object\>(Attribute Name: data) |               |          |                                                              |                                   |
| \<list\>(Attribute Name: orders) |               |          |                                                              |                                   |
| order_id                         | true          | long     | Order ID                                                     |                                   |
| symbol                           | true          | string   | Variety code                                                 |                                   |
| contract_type                    | true          | string   | Contract Type                                                | "this_week","next_week","quarter" |
| contract_code                    | true          | string   | Contract Code                                                | "BTC180914" ...                   |
| lever_rate                       | true          | int      | Leverage Rate                                                | 1\\5\\10\\20                      |
| direction                        | true          | string   | Transaction direction                                        |                                   |
| offset                           | true          | string   | "open": "close"                                              |                                   |
| volume                           | true          | decimal  | Number of Order                                              |                                   |
| price                            | true          | decimal  | Price committed                                              |                                   |
| create_date                      | true          | long     | Creation time                                                |                                   |
| order_source                     | true          | string   | Order Source                                                 |                                   |
| order_price_type                 | true          | string   | Order price type [limited price，opponent price，market price] |                                   |
| margin_frozen                    | true          | decimal  | Freeze margin                                                |                                   |
| profit                           | true          | decimal  | profit                                                       |                                   |
| trade_volume                     | true          | decimal  | Transaction quantity                                         |                                   |
| trade_turnover                   | true          | decimal  | Transaction aggregate amount                                 |                                   |
| fee                              | true          | decimal  | Servicefee                                                   |                                   |
| trade_avg_price                  | true          | decimal  | Transaction average price                                    |                                   |
| status                           | true          | int      | Order Status                                                 |                                   |
| \</list\>                        |               |          |                                                              |                                   |
| \</object\>                      |               |          |                                                              |                                   |
| total_page                       | true          | int      | Total Pages                                                  |                                   |
| current_page                     | true          | int      | Current Page                                                 |                                   |
| total_size                       | true          | int      | Total Size                                                   |                                   |
| ts                               | true          | long     | Timestamp                                                    |                                   |

**Example**
```
POST https://api.hbdm.com/api/v1/contract_hisorders

# Response
{
  "status": "ok",
  "data":{
    "orders":[
      {
        "symbol": "BTC",
        "contract_type": "this_week",
        "contract_code": "BTC180914",
        "volume": 111,
        "price": 1111,
        "order_price_type": "limit",
        "direction": "buy",
        "offset": "open",
        "lever_rate": 10,
        "order_id": 106837,
        "client_order_id": 10683,
        "order_source": "web",
        "created_at": 1408076414000,
        "trade_volume": 1,
        "trade_turnover": 1200,
        "fee": 0,
        "trade_avg_price": 10,
        "margin_frozen": 10,
        "profit": 10,
        "status": 1
      }
     ],
    "total_page":15,
    "current_page":3,
    "total_size":3
    },
  "ts": 1490759594752
}
```