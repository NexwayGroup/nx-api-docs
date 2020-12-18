
## Get a list of templates
### Request

`GET https://api.nexway.store/tandcs/templates`

### Query parameters

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|page| 	int32|number of page you want to hit||	
|size| 	int32|number of items into the current page||
|sort| 	string|sort a specific column to a direction (columnName,asc)||

### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
