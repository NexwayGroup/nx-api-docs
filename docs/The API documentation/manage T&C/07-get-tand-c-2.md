
## Get a terms and conditions by customer Id and locale
### Request

`GET https://api.nexway.store/tandcs/public/tandcs/{customerAndPlaftormId}/{locale}`

### Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|customerAndPlaftormId| 	string|customerAndPlaftormId|	Required|
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