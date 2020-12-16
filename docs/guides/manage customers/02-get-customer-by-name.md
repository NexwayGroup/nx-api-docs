
## Get a customer by name 
### Request

`GET https://api.nexway.store/customers/name/{name}`

### Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|name| 	string|name|	Required|

### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	[Customer](https://api-doc.nexway.store/nexway-monetize/resources/customer?v=latest)|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||