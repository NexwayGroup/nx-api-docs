
## Get a store by ID

Get a store entry based on an ID
### Request

`GET https://api.nexway.store/stores/{id}`

### Path parameters

|Parameter name| 	Value| 	Description |	Additional|
|--|--|--|--|
|id |	string 	|id|	Required|

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	[Store](/docs/guides/manage%20stores/07-store.md)|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
