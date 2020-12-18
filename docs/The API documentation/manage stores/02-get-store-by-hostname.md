
## Get a store by hostname

Get a store entry based on a hostname
### Request

`GET https://api.nexway.store/stores/hostname/{hostname}`

### Path parameters

|Parameter name| 	Value| 	Description |	Additional|
|--|--|--|--|
|hostname |	string 	|hostname|	Required|

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	[Store](/docs/The%20API%20Documentation/manage%20stores/07-store.md)|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
