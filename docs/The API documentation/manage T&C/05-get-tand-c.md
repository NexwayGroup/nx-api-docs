
## Get a T&C
### Request

`GET https://api.nexway.store/tandcs/tandcs/{platform}/{customerId}/current`

### Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|platform 	|string|platform|	Required|
|customerId| 	string|customerId|	Required|

### Query parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|withTC| 	boolean|withTC||
	
### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success |[TermsAndConditions](/docs/The%20API%20Documentation/manage%20T&C/16-termsAndConditions.md)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||