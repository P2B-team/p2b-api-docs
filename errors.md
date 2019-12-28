
**Errors:**

Category | Error code | Status Code | Response message | Description
------------ | ------------ | ------------ | ------------ | ------------
  auth | 1001  | 401 | Key not provided. | X-TXC-APIKEY header is missing in the request or empty.
  auth | 1002  | 401 | Payload not provided. | X-TXC-PAYLOAD header is missing in the request or empty.
  auth | 1003  | 401 | Signature not provided. | X-TXC-SIGNATURE header is missing in the request or empty.
  auth | 1004  | 401 | Nonce and url not provided. | Request body is empty. Missing required parameters "request", "nonce".
  auth | 1005  | 401 | Invalid body data. | Invalid request body
  auth | 1006  | 401 | Nonce not provided. | Request body missing required parameter "nonce".
  auth | 1007  | 401 | Request not provided. | Request body missing required parameter "request".
  auth | 1008  | 401 | Invalid request in body. | The passed request parameter does not match the URL of this request.
  auth | 1009  | 401 | Invalid payload. | The transmitted payload value (X-TXC-PAYLOAD header) does not match the request body.
  auth | 1010  | 401 | This action is unauthorized. | - API key passed in the X-TXC-APIKEY header does not exist. - Access to API is not activated. Go to profile and activate access.
  auth | 1011  | 401 | This action is unauthorized. Please, enable two-factor authentication. | Two-factor authentication is not activated for the user.
  auth | 1012  | 401 | Invalid nonce. | Parameter "nonce" is not a number.
  auth | 1013  | 429 | Too many requests. | - A request came with a repeated value of nonce. - Received more than the limited value of requests (10) within one second.
  auth | 1014  | 401 | Unauthorized request. | Signature value passed (in the X-TXC-SIGNATURE header) does not match the request body.
  auth | 1015  | 423 | Temporary block. | Temporary blocking. There is a cancellation of orders.
  auth | 1016  | 401 | Not unique nonce. | The request was sent with a repeated parameter "nonce" within 10 seconds.
  
 Category | Error code | Status Code | Response message | Description
 ------------ | ------------ | ------------ | ------------ | ------------ 
  data | 2010  | 400 | Currency not found. | Currency not found.
  data | 2020  | 400 | Market is not available. | Market is not available.
  data | 2021  | 400 | Unknown market. | Unknown market.
  data | 2030  | 400 | Order not found. | Order not found. 
  data | 2040  | 400 | Balance not enough. | Insufficient balance.
  data | 2050  | 400 | Amount less than the permitted minimum. | Amount less than the permitted minimum.
  data | 2051  | 400 | Amount is greater than the maximum allowed. | Amount exceeds the allowed maximum.
  data | 2052  | 400 | Amount step size error. | Amount step size error. 
  data | 2060  | 400 | Price less than the permitted minimum. | Price is less than the permitted minimum.
  data | 2061  | 400 | Price is greater than the maximum allowed. | Price exceeds the allowed maximum.
  data | 2062  | 400 | Price pick size error. | Price pick size error.
  data | 2070  | 400 | Total less than the permitted minimum. | Total less than the permitted minimum.

  
  Category | Error code | Status Code | Response message | Description
  ------------ | ------------ | ------------ | ------------ | ------------
  validation | 3001  | 422 | Validation exception. The given data was invalid.  | 
  validation | 3020  | 422 | Invalid currency value.  |  Incorrect parameter, check your request.
  validation | 3030  | 422 | Invalid market value. | Incorrect "market" parameter, check your request.
  validation | 3040  | 422 | Invalid amount value. | Incorrect "amount" parameter, check your request.
  validation | 3050  | 422 | Invalid price value. | Incorrect "price" parameter, check your request.
  validation | 3060  | 422 | Invalid limit value. | Incorrect "limit" parameter, check your request.
  validation | 3070  | 422 | Invalid offset value. | Incorrect "offset" parameter, check your request.
  validation | 3080  | 422 | Invalid orderId value. | Incorrect "orderId" parameter, check your request.
  validation | 3090  | 422 | Invalid lastId value. | Incorrect "lastId" parameter, check your request.
  validation | 3100  | 422 | Invalid side value. | Incorrect "side" parameter, check your request.
  validation | 3110  | 422 | Invalid interval value. | Incorrect "interval" parameter, check your request.

Category | Error code | Status Code | Response message | Description
------------ | ------------ | ------------ | ------------ | ------------
  service | 4001  | 500 | Service temporary unavailable.  |  An unexpected system error has occurred. Try again after a while. If the error persists, please contact support.