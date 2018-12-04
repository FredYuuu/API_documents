目前关于apikey申请和修改，请在“账户 - API管理”页面进行相关操作。

支持所有**Huobi Global**中的交易对

常见问题请参考[FAQ](https://github.com/huobiapi/API-FAQ/wiki)

欢迎有优秀 maker 策略交易量大的用户参与长期做市商项目。如果您的火币账户中有折合大于20BTC资产，请提供以下信息到MM_service@huobi.com（做市商项目不支持点卡抵扣、VIP、交易量相关活动以及任何形式的返佣活动）:

> 1. 提供uid（需不存在返佣关系的uid）
> 2. 提供其他交易平台证明maker交易量截图（比如30天内成交量，或者VIP等级等）

# 现货与杠杆

## WebSocket行情，资产和交易推送API<br>
 >  **适用于 Huobi Global**<br>

* [WebSocket API简介](WS_introduction)<br>
* [请求与订阅说明](WS_request)<br>
* [API Reference](WS_api_reference)<br>
* [错误代码](WS_error_code)<br>
* 代码示例：[Python3](https://github.com/huobiapi/Websocket-Python3-demo)  [Node.js](https://github.com/huobiapi/WebSocket-Node.js-demo)  [PHP](https://github.com/huobiapi/WebSocket-PHP-demo) 
 [CSharp](https://github.com/huobiapi/WebSocket-CSharp-demo) 

## REST行情、交易API<br>
 >  **适用于 Huobi Global**<br>

* [REST API简介](REST_introduction)<br>
* [签名认证(重要，请仔细阅读)](REST_authentication)<br>
* [请求说明(一定要看)](REST_request)<br>
* [API Reference](REST_api_reference)<br>
* [错误代码](REST_error_code)<br>
* 代码示例 [Python3](https://github.com/huobiapi/REST-Python3-demo) [Node.js](https://github.com/huobiapi/REST-Node.js-demo) [Java](https://github.com/huobiapi/REST-Java-demo) [C#](https://github.com/huobiapi/REST-CSharp-demo) [go](https://github.com/huobiapi/REST-GO-demo) [PHP](https://github.com/huobiapi/REST-PHP-demo) [C++](https://github.com/huobiapi/REST-Cpp-demo) [Objective-C](https://github.com/huobiapi/REST-ObjectiveC-demo) [QTC++](https://github.com/huobiapi/REST-QTCpp-demo) [Python2.7](https://github.com/huobiapi/REST-Python2.7-demo) [Ruby](https://github.com/huobiapi/REST-Ruby-demo) [易语言](https://github.com/huobiapi/REST-YiYuyan-demo)

## 子账号API相关说明

* 子账号 API Key 与现有母账户安全策略一致；
* 子账号开放的接口包括全部行情接口以及如下需要验签的接口。其他接口子账号不可访问，如果尝试访问，系统会返回error-code 403。

接口|说明|
----------------------|---------------------|
[POST /v1/order/orders/place](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#post-v1orderordersplace-pro%E7%AB%99%E4%B8%8B%E5%8D%95)	|创建并执行订单|
[POST /v1/order/orders/{order-id}/submitcancel](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#post-v1orderordersorder-idsubmitcancel--%E7%94%B3%E8%AF%B7%E6%92%A4%E9%94%80%E4%B8%80%E4%B8%AA%E8%AE%A2%E5%8D%95%E8%AF%B7%E6%B1%82)	|撤销一个订单|
[POST /v1/order/orders/batchcancel](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#post-v1orderordersbatchcancel-%E6%89%B9%E9%87%8F%E6%92%A4%E9%94%80%E8%AE%A2%E5%8D%95)	|批量撤销订单|
[POST /v1/order/orders/batchCancelOpenOrders](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#post--v1orderordersbatchcancelopenorders--%E6%89%B9%E9%87%8F%E5%8F%96%E6%B6%88%E7%AC%A6%E5%90%88%E6%9D%A1%E4%BB%B6%E7%9A%84%E8%AE%A2%E5%8D%95)	|撤销当前委托订单|
[GET /v1/order/orders/{order-id}](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1orderordersorder-id-%E6%9F%A5%E8%AF%A2%E6%9F%90%E4%B8%AA%E8%AE%A2%E5%8D%95%E8%AF%A6%E6%83%85)	|查询一个订单详情|
[GET /v1/order/orders](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1orderorders-%E6%9F%A5%E8%AF%A2%E5%BD%93%E5%89%8D%E5%A7%94%E6%89%98%E5%8E%86%E5%8F%B2%E5%A7%94%E6%89%98)	|查询当前委托、历史委托|
[GET /v1/order/openOrders](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1orderopenorders-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E5%BD%93%E5%89%8D%E5%B8%90%E5%8F%B7%E4%B8%8B%E6%9C%AA%E6%88%90%E4%BA%A4%E8%AE%A2%E5%8D%95)	|查询当前委托订单|
[GET /v1/order/matchresults](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1ordermatchresults-%E6%9F%A5%E8%AF%A2%E5%BD%93%E5%89%8D%E6%88%90%E4%BA%A4%E5%8E%86%E5%8F%B2%E6%88%90%E4%BA%A4)	|查询成交|
[GET /v1/order/orders/{order-id}/matchresults](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1orderordersorder-idmatchresults--%E6%9F%A5%E8%AF%A2%E6%9F%90%E4%B8%AA%E8%AE%A2%E5%8D%95%E7%9A%84%E6%88%90%E4%BA%A4%E6%98%8E%E7%BB%86)	|查询某个订单的成交明细|
[GET /v1/account/accounts](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1accountaccounts)	|查询当前用户的所有账户|
[GET /v1/account/accounts/{account-id}/balance](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference#get-v1accountaccountsaccount-idbalance-%E6%9F%A5%E8%AF%A2pro%E7%AB%99%E6%8C%87%E5%AE%9A%E8%B4%A6%E6%88%B7%E7%9A%84%E4%BD%99%E9%A2%9D)	|查询指定账户的余额|

# 合约

## WebSocket 行情,交易 API<br>
 >  **适用于 火币合约站 hbdm.com**<br>
* [API Reference](WS_api_reference_Derivatives)<br>
* [错误代码](WS_error_code_derivatives)<br>
* 代码示例：[Node.js](https://github.com/huobiapi/Futures-Node.js-demo)<br>

## REST 行情、交易 API<br>
 >  **适用于 火币合约站 hbdm.com**<br>
* [API Reference](REST_api_reference_Derivatives)<br>
* [错误代码](WS_error_code_derivatives)<br>
* 代码示例：[Python](https://github.com/huobiapi/Futures-Python-demo)  [Java](https://github.com/huobiapi/Futures-Java-demo) [Node.js](https://github.com/huobiapi/Futures-Node.js-demo) [PHP](https://github.com/huobiapi/Futures-PHP-demo)<br>

English document [click here](https://github.com/huobiapi/API_Docs_en/wiki)
