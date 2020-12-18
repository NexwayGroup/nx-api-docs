
## Get a terms and conditions by plaform and customer Id and locale and etc
### Request

`GET https://api.nexway.store/tandcs/public/tandcs/{platform}/{customerId}/{locale}`

## Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|platform |	string|platform|	Required|
|customerId| 	string|customerId|	Required|
|locale| 	string|locale|	Required|

### Query parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|date| 	string|date||

### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
