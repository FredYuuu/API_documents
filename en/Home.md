Please sign in huobi.pro and then visit “Account - API Management” to manage your apikeys.

All of the trading pairs in huobi.pro and hadax.com are supported.

The root url is: 

Pro： api.huobi.pro

HADAX：api.hadax.com

Eligibility Criteria as a Market Maker on Huobi Global
> 1. You should possess good market strategy;
> 2. You must have at least 20 BTC or equivalent assets, not including rebates in your account with Huobi Global;
> 3. You will not be entitled to any Point card, VIP or rebate programs;
> 4. You will be subjected to performance evaluation at end of each calendar month by our ranking mechanism whereby the ranking is based on your average daily volume. You will be disqualified as a market-maker if your ranking remained the lowest at the end of each month.

Application Process as Market Maker on Huobi Global
If you comply with our eligibility criteria and is interested to participate in our market-making project, please email to MM_service@huobi.com with following information:
> 1. UIDs (not linked to any rebate program in any accounts);
> 2. Provide screenshot of trading volume for the past 30 days or VIP/corporate status with other Exchanges;
> 3. A short presentation of your market-making strategy.

# Websocket API(Market & Trade)

* [General](WS_General)
* [Reference](WS_Reference)
* Demo:[Python3](https://github.com/huobiapi/Websocket-Python3-demo)  [Node.js](https://github.com/huobiapi/WebSocket-Node.js-demo)  [PHP](https://github.com/huobiapi/WebSocket-PHP-demo) 
 [CSharp](https://github.com/huobiapi/WebSocket-CSharp-demo) 

# REST API(Market & Trade)

* [Signing API Requests(Important)](Signing_API_Requests)
* [Request Process](Request_Process)
* [Reference](REST_Reference)
* Demo:[Python3](https://github.com/huobiapi/REST-Python3-demo) [Node.js](https://github.com/huobiapi/REST-Node.js-demo) [Java](https://github.com/huobiapi/REST-Java-demo) [C#](https://github.com/huobiapi/REST-CSharp-demo) [go](https://github.com/huobiapi/REST-GO-demo) [PHP](https://github.com/huobiapi/REST-PHP-demo) [C++](https://github.com/huobiapi/REST-Cpp-demo) [Objective-C](https://github.com/huobiapi/REST-ObjectiveC-demo) [QTC++](https://github.com/huobiapi/REST-QTCpp-demo) [Python2.7](https://github.com/huobiapi/REST-Python2.7-demo) [Ruby](https://github.com/huobiapi/REST-Ruby-demo) [易语言](https://github.com/huobiapi/REST-YiYuyan-demo)

# SUB-UID API Usage
* An API Key of a sub users could not be linked to IP addresses, so it will be expired in 90 days.
* Besides all open Market Data APIs, following APIs, which require signature are available for sub users. When sub users tries to access the other APIs not on this list, the system will return error-code 403.  

Request Mehtod|Description|
----------------|-----------------------|
[POST/v1/order/orders/place](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#post-v1orderordersplace--make-an-order-in-huobipro)|	Place an order |
[POST/v1/order/orders/{order-id}/submitcancel](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#post-v1orderordersorder-idsubmitcancel--request-for-cancelling-an-order)	| Request to cancel an order |
[POST /v1/order/orders/batchcancel](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#post-v1orderordersbatchcancel--batch-cancel)|	Request to cancel a batch of orders, up to 50 orders |
[POST /v1/order/orders/batchCancelOpenOrders](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#post--v1orderbatchcancelopenorders--cancel-a-batch-of-orders-with-certain-criteria)	 |Request to cancel a batch of orders, which meet certain criteria, up to 100 orders |
[GET /v1/account/accounts](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1accountaccounts-get-all-the-accounts-pro-and-hadax-share-the-same-account-id)	| get the status of an account|
[GET /v1/account/accounts/{account-id}/balance](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1accountaccountsaccount-idbalance-----get-balance-in-huobipro)	| Get the balance of an account |
[GET /v1/order/orders/{order-id}](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1orderordersorder-id----get-order-info)	|Get the details of an order|
[GET /v1/order/orders/{order-id}/matchresults](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1orderordersorder-idmatchresults--get-order-matchresult) 	 |Get detail match results of an order |
[GET /v1/order/orders](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1orderorders--get-order-list) |	Search for a group of orders, which meet certain criteria (up to 100) |
[GET /v1/order/matchresults](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1ordermatchresults----get-order-matchresults) |	Search for the trade records of an account|
[GET /v1/order/openOrders](https://github.com/huobiapi/API_Docs_en/wiki/REST_Reference#get-v1orderopenorders-provide-open-orders-of-a-symbol-for-an-account) |	Get the open orders of an account (up to 500)|


# hbdm.com
## Websocket API(Market)

* [Reference](https://github.com/huobiapi/API_Docs_en/wiki/WS_api_reference_en)
* Demo：[Python](https://github.com/huobiapi/Futures-Python-demo) [Node.js](https://github.com/huobiapi/Futures-Node.js-demo)<br>

## REST API(Market & Trade)

* [Reference](https://github.com/huobiapi/API_Docs_en/wiki/API_Reference_Derivatives_en)
* Demo：[Python](https://github.com/huobiapi/Futures-Python-demo)  [Java](https://github.com/huobiapi/Futures-Java-demo) [Node.js](https://github.com/huobiapi/Futures-Node.js-demo) [PHP](https://github.com/huobiapi/Futures-PHP-demo)<br>



