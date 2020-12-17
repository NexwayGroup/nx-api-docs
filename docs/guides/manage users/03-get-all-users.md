
## Get list of users

Retrieved all users from a customer.
### Request

`GET https://api.nexway.store/iam/users`

### Query parameters
|Parameter name| 	Value| 	Description|
|--|--|--|
|sort| 	string|sort a specific column to a direction (columnName,asc)|
|page| 	int32|number of page you want to hit|
|size| 	int32|number of items into the current page|
|userName| 	string|filter the users by user name which contain the given value|
|firstName| 	string|filter the users by first name which contain the given value|
|lastName| 	string|filter the users by last name which contain the given value|
|email| 	string|filter the users by email which contain the given value|
|status| 	string|filter the users by status|
|creationDate| 	string|filter the users by creation date with format yyyy-MM-dd'T'HH:mm:ss. The < symbol allow to filter before (< X), after (X <) and between a range of dates (X << Y)|
	
### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|200| 	**OK** Success |[UserSearchResult](/docs/nx-api-docs/docs/guides/manage%20users/07-userSearchResult.md)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||