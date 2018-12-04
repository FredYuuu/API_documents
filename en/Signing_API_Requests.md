Important Note: Do not reveal your 'AccessKey' and 'SecretKey' to anyone. They are as important as your password. Pro and HADAX share the same KEYs.

## Components of a Query Request

Huobi requires that each HTTPS request formatted for Signature should contain the following:

* Path:api.huobi.pro or api.hadax.com, and the method followed like 'api.huobi.pro/v1/order/orders'.
* AccessKeyId: The 'AccessKey' distributed by Huobi when you applied for APIKEY.
* SignatureMethod: The hash-based protocol used to calculate the signature. This should be HmacSHA256.
* SignatureVersion: The version of the signature protocol.This shoule be 2.
* Timestamp: The time at which you make the request. Include this in the Query request to help prevent third parties from intercepting your request.This should be formatted in UTC time, like '2017-05-11T16:22:06'.
* Required and optional parameters: Each action has a set of required and optional parameters that define the API call.This shows in the API reference.Important Reminder: Every param in a GET method should be included in signature calculation, and only AccessKeyId, SignatureMethod, SignatureVersion, Timestamp should be included in signature calculation for a POST method.
* Signature: The calculated value that ensures the signature is valid and is not tampered.

For example:
```
https://api.huobi.pro/v1/order/orders?
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T15%3A19%3A30
&order-id=1234567890
&Signature=calculated value
```

## How to Generate a Signature

Web service requests are sent across the Internet and are vulnerable to tampering. For security reasons,  Huobi requires a signature as part of every request.

### Task 1: Format the Query Request

Before you can sign the Query request,  normalize the request in a standardized (canonical) format. This is necessary because different formats will result in different HMAC signatures. Normalize the request in a canonical format before signing. This ensures your application and Huobi server will calculate the same signature for a request.

The following example generates the string to sign for the following call to the Huobi server.

```
https://api.huobi.pro/v1/order/orders?
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
&SignatureMethod=HmacSHA256
&SignatureVersion=2
&Timestamp=2017-05-11T15:19:30
&order-id=1234567890
```

1. Start with the request method (either GET or POST),  followed by a newline character. For human readability,  the newline character is represented as \n.

```
GET\n
```

2. Add the root url in lowercase,  followed by a newline character.

```
api.huobi.pro\n
```

3. Add the method,  followed by a newline character. 

```
/v1/order/orders\n
```

4. Add the query string components,  as UTF-8 characters which are URL encoded (hexadecimal characters must be uppercase). Please do not encode the initial question mark character (?) in the request.Then sort the query string components by byte order. Byte ordering is case sensitive.

For example, this is the original order for the query string components.

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
order-id=1234567890
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
```
The query string components would be reorganized as the following:

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx
SignatureMethod=HmacSHA256
SignatureVersion=2
Timestamp=2017-05-11T15%3A19%3A30
order-id=1234567890
```

5. Separate parameter names from their values with the equal sign character (=),  even if the value is empty. Separate parameter and value pairs with the ampersand character (&). Concatenate the parameters and their values to make one long string with no spaces. Spaces within a parameter value are allowed,  but must be URL encoded as %20. In the concatenated string,  period characters (.) are not escaped. 

The following example shows the query string components,  with the parameters concatenated with the ampersand character (&),  and sorted by byte order.

```
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```
To construct the finished canonical request,  combine all the components from each step. As shown,  each component ends with a newline character.

```
GET\n
api.huobi.pro\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```

Task 2: Calculate the Signature

1. The signature is calculated with the following canonical string and secret key as inputs to a keyed hash function:

* Canonical query string:

```
GET\n
api.huobi.pro\n
/v1/order/orders\n
AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&order-id=1234567890
```

*  Secret key(The 'SecretKey' distributed by Huobi when you applied for APIKEY.):

```
b0xxxxxx-c6xxxxxx-94xxxxxx-dxxxx
```

* The resulting signature must be base-64 encoded.

```
4F65x5A2bLyMWVQj3Aqp+B4w+ivaA7n5Oi2SuYtCJ9o=
```

2. Add the resulting value to the query request as a Signature parameter. When you add this parameter to the request,  you must URI encode it just like any other parameter. You can use the signed request in an HTTP or HTTPS call.

3. The final request ：

```
https://api.huobi.pro/v1/order/orders?AccessKeyId=e2xxxxxx-99xxxxxx-84xxxxxx-7xxxx&order-id=1234567890&SignatureMethod=HmacSHA256&SignatureVersion=2&Timestamp=2017-05-11T15%3A19%3A30&Signature=4F65x5A2bLyMWVQj3Aqp%2BB4w%2BivaA7n5Oi2SuYtCJ9o%3D
```
