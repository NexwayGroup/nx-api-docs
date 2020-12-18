
### TokenRequest resource
## Methods

* [post](/docs/The%20API%20Documentation/JWT%20Authentication/02-get-user-token.md) - Get/refresh a token

## Resource

```json
{
    "clientSecret": "string",
    "grantType": "string",
    "password": "string",
    "realmName": "string",
    "username": "string"
}
```

## Properties
| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|clientSecret |	string |	Client secret | Optional |
|grantType |	string | Request type. Possible values are: `password`, `refresh_token`, `client_credentials` | Optional |
|password |	string |	Password | Optional |
|realmName |	string 	| Realm name |  |
|username |	string | User name | Optional|
