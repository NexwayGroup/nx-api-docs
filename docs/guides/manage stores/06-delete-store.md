

## Delete a store

Delete an existing store entry

### Request

`DELETE https://api.nexway.store/stores/{id}`

### Path parameters

|Parameter name| 	Value| 	Description |	Additional|
|--|--|--|--|
|id |	string 	|id|	Required|
	
### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|204| 	**No Content** Success ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
