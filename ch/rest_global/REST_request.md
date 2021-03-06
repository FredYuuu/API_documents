##  请求说明

### 访问地址

**注意：<br>**
**_1.请使用中国大陆以外的服务器访问火币 API；_<br>**
**_2.HADAX和Pro是两个不同的交易站，所以行情信息也不一样，请通过 [Pro symbols](http://api.huobi.pro/v1/common/symbols) 和 [HADAX symbols](https://api.hadax.com/v1/common/symbols) 查询相应的交易对信息。_**


- Pro 站：
行情： https://api.huobi.pro/market
交易： https://api.huobi.pro/v1

- HADAX 站：
行情： https://api.hadax.com/market
交易： https://api.hadax.com/v1

- 合约：
行情/交易： https://api.hbdm.com

* POST请求头信息中必须声明 Content-Type:application/json;GET请求头信息中必须声明 Content-Type:application/x-www-form-urlencoded。(汉语用户建议设置 Accept-Language:zh-cn)
* 所有请求参数请按照 API 说明进行参数封装。
* 将封装好参数的 API 请求通过 POST 或 GET 的方式提交到服务器。
* 火币网处理请求，并返回相应的 JSON 格式结果。
* 请使用 https 请求。
* 限制频率为10秒100次（单个APIKEY维度限制，建议行情API访问也要加上签名，否则限频会更严格）。
* 查询资产详情方法调用顺序：查询当前用户的所有账户->查询指定账户的余额 
* 支持所有Pro站上交易中的交易对，上新币保持与网站同步。