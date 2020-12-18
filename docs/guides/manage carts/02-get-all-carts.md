
### Get list of carts
## Request

`GET https://api.nexway.store/carts`

### Query parameters

|Name| 	Type| 	Description|
|--|--|--|
|page| 	int32|number of page you want to hit|
|size| 	int32|number of items into the current page|
|sort| 	string|sort a specific column to a direction (columnName,asc)|
|storeId| 	string|filter the carts by store ID|
|id| 	string|filter the carts by ID. The * symbol may substitute letters at the beginning and at the end|
|customerId| 	string|filter the carts by customer ID. The * symbol may substitute letters at the beginning and at the end|
|status| 	ref|filter the carts by status|

### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description| 	Resource|
|--|--|--|
|200| 	**OK** Success |	[CartSearchResult](/docs/nx-api-docs/docs/guides/manage%20carts/05-cartSearchResult.md)|
|401| 	**Unauthorized** Unauthorized| |	
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
