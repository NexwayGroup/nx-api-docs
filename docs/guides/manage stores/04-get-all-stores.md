
## Get list of stores

Get all stores with pagination
### Request

`GET https://api.nexway.store/stores`

### Query parameters

|Name| 	Type| 	Description|
|--|--|--|
|page| 	int32|Number of page you want to display|
|size| 	int32|Number of items to load per page|
|sort| 	string|Sort column (ascending/descending)|
|id| 	string|Filter stores by ID|
|creationDate| 	string|Filter stores by creation date with format yyyy-MM-dd'T'HH:mm:ss; the < symbol allows filtering before (< X), after (X <), and between a range of dates (X << Y)|
|status| 	ref|Filter stores by status|
|name| 	string|Filter stores by name; you can use the * symbol to substitute for letters at the beginning and at the end|
|customerId| 	string|Filter stores by customer ID.|
|baseProduct| 	string 	|Filter stores by base product ID; you can use the * symbol to substitute for letters at the beginning and at the end|
|defaultLocale| 	string|Filter stores by default locale; you can use the * symbol to substitute for letters at the beginning and at the end|
|theme| 	string|Filter stores by theme; you can use the * symbol to substitute for letters at the beginning and at the end|
|status| 	ref|Filter stores by status|

### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success |[SearchResult«Store»](https://api-doc.nexway.store/nexway-monetize/resources/searchresultstore?v=latest)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
