
## Delete a template variable
### Request

`DELETE https://api.nexway.store/tandcs/variables/{id}`

### Path parameters

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|id| 	string|id|	Required|

### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success ||
|204| 	**No Content** No Content ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
