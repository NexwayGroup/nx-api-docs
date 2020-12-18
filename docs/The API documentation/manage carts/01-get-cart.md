
## Get a cart by id
### Request

`GET https://api.nexway.store/carts/{id}`

### Path parameters

|Parameter name| 	Value| 	Description| 	Additional|
|--|--|--|--|
|id| 	string|id|	Required|

### Query parameters

|Name| 	Type| 	Description|
|--|--|--|
|readyForOrder| 	boolean|readyForOrder|
|version| 	int64|version|
|endUserIp| 	string|endUserIp|

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	[Cart](/docs/The%20API%20Documentation/manage%20carts/04-cart.md)|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
