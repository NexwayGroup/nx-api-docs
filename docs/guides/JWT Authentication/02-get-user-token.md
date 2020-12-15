
## Get/refresh a token

Get a JWT token or refresh a new one.
### Request

`POST https://api.nexway.store/iam/tokens`

### Request body

The request body takes a complete TokenRequest resource, containing the following writable properties:

```json
{
    "clientSecret": "string",
    "grantType": "string",
    "password": "string",
    "realmName": "string",
    "username": "string"
}
```

### Properties
| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|clientSecret |	string |	Client secret | Optional |
|grantType |	string | Request type. Possible values are: `password`, `refresh_token`, `client_credentials` | Optional |
|password |	string |	Password | Optional |
|realmName |	string 	| Realm name |  |
|username |	string | User name | Optional|

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	AccessTokenResponse|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||

