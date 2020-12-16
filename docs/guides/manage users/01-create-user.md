
## Create a user

Create a user from a customer.
### Request

`POST https://api.nexway.store/iam/users`

## Request body

The request body takes a complete [User resource](https://api-doc.nexway.store/nexway-monetize/resources/user?v=latest), containing the following writable properties:
```json
{
    "authorizedCustomers": [
        {
            "customerId": "string",
            "name": "string"
        }
    ],
    "email": "string",
    "emails": null,
    "firstName": "string",
    "lastName": "string",
    "password": "string",
    "roles": [
        {
            "clientRole": "boolean",
            "composite": "boolean",
            "composites": {
                "application": {
                    "<key>": [
                        "string"
                    ]
                },
                "client": {
                    "<key>": [
                        "string"
                    ]
                },
                "realm": [
                    "string"
                ]
            },
            "containerId": "string",
            "description": "string",
            "id": "string",
            "name": "string",
            "scopeParamRequired": "boolean"
        }
    ],
    "status": "string",
    "userName": "string"
}
```
### Properties
|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|authorizedCustomers[]| 	array|List of authorized customer|	Optional|
|authorizedCustomers[].customerId| 	string|Customer id|	Optional|
|authorizedCustomers[].name| 	string|Customer name|	Optional|
|email| 	string|Email||
|emails[]| 	array|List of emails sent to the user|Optional|
|firstName| 	string|First name||	
|lastName| 	string|Last name||
|password| 	string|Password||
|roles[]| 	array|List of roles|Optional|
|roles[].clientRole| 	boolean|| 		Optional|
|roles[].composite| 	boolean|| 		Optional|
|roles[].composites| 	object| 	Composites| 	Optional|
|roles[].composites.application| 	object| 		|Optional|
|roles[].composites.application.<key\>[]| 	array of string|| 		Optional|
|roles[].composites.client| 	object|| 		Optional|
|roles[].composites.client.<key\>[]| 	array of string|| 		Optional|
|roles[].composites.realm[]| 	array of string|| 		Optional|
|roles[].containerId| 	string|| 		Optional|
|roles[].description| 	string|| 		Optional|
|roles[].id| 	string|| 		Optional|
|roles[].name| 	string|| 		Optional|
|roles[].scopeParamRequired| 	boolean|| 		Optional|
|status| 	string|Status, possible values are: `ENABLED`,`DISABLED`|	Optional|
|userName| 	string|Name||
	
### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/nx-api-docs/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|201| 	**Created** Success |[Success](https://api-doc.nexway.store/nexway-monetize/resources/success?v=latest)|
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||