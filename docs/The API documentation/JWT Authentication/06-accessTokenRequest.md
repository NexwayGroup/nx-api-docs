
### AccessTokenResponse resource
## Methods

* [post](/docs/The%20API%20Documentation/JWT%20Authentication/02-get-user-token.md) - Get/refresh a token

## Resource

```json
{
    "access_token": "string",
    "expires_in": "int64",
    "id_token": "string",
    "not-before-policy": "int32",
    "refresh_expires_in": "int64",
    "refresh_token": "string",
    "session_state": "string",
    "token_type": "string"
}
```

## Properties

| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|access_token| 	string|| 		Optional|
|expires_in| 	int64| 		|Optional|
|id_token| 	string|| 		Optional|
|not-before-policy| 	int32|| 		Optional|
|refresh_expires_in| 	int64|| 		Optional|
|refresh_token| 	string|| 		Optional|
|session_state| 	string|| 		Optional|
|token_type| 	string|| 		Optional| 
