
## Get a user by id

Retrieved one user from a customer
### Request

`GET https://api.nexway.store/iam/users/{id}`

### Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|id| 	string|id|	Required|

### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success |[User](/docs/guides/manage%20users/06-user.md)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||