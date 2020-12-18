
## Update a template
### Request

`PUT https://api.nexway.store/tandcs/templates/{id}`

### Path parameters

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|id| 	string|id|	Required|

### Request body

The request body takes a complete Template resource, containing the following writable properties:

```json
{
    "createDate": "date-time",
    "dbVersion": "int64",
    "id": "string",
    "lastUpdater": "string",
    "locale": "string",
    "startDate": "date-time",
    "status": "string",
    "subject": "string",
    "template": "string",
    "updateDate": "date-time"
}
```
### Properties

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|createDate| 	date-time|| 		Optional|
|dbVersion 	|int64| 		|Optional|
|id| 	string|| 		Optional|
|lastUpdater| 	string|| 		Optional|
|locale| 	string|| 		Optional|
|startDate| 	date-time|| 		Optional|
|status| 	string|Possible values are:`RUNNING`,`ARCHIVED`,`DRAFT`|	Optional|
|subject| 	string|| 		Optional|
|template| 	string|| 		Optional|
|updateDate| 	date-time| 		|Optional|

### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success |[Template](/docs/The%20API%20Documentation/manage%20T&C/15-template.md)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
