1. The root url is: 

Pro : 'https://api.huobi.pro/market' for marketing and 'https://api.huobi.pro/v1' for trading.

HADAX: 'https://api.hadax.com/market' for marketing and 'https://api.hadax.com/v1' for trading.


2. The field 'Content-Type' in request header should be 'application/x-www-form-urlencoded' for a GET method, and 'application/x-www-form-urlencoded' for a POST method. 
3. Request parameter: Construct the request parameters according to each API interface.
4. Submit request: Use POST or GET method to send request to server.
5. HTTPS is required.
6. Server respond: Server processes request and returns data to users in JSON format after authentication check.
7. Each apikey can send maximum of 100 https requests within 10 seconds.
8. All of the trading pairs in huobi.pro are supported.