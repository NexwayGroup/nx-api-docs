
## Invalidate a token

Invalidate a token from a user during a logout process
### Request

`DELETE https://api.nexway.store/iam/tokens/invalidate`

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|
|--|--|
|200| 	**OK** OK|
|204| 	**No Content** No content|
|401| 	**Unauthorized** Unauthorized|
|403| 	**Forbidden** Forbidden|
|404| 	**Not Found** Not Found|
|500| 	**Internal Server Error** Failure|