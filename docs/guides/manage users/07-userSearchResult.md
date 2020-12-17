
## UserSearchResult resource

### Methods

- [get](/docs/nx-api-docs/docs/guides/manage%20users/03-get-all-user.md) - Get list of users

### Resource

```json
{
    "items": [
        {
            "authorizedCustomers": [
                {
                    "customerId": "string",
                    "name": "string"
                }
            ],
            "createdAt": "date-time",
            "email": "string",
            "emails": [
                {
                    "createDate": "date-time",
                    "emailId": "string",
                    "error": "string",
                    "id": "string",
                    "type": "string"
                }
            ],
            "firstName": "string",
            "id": "string",
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
            "token": "string",
            "userName": "string"
        }
    ],
    "last": "boolean",
    "number": "int32",
    "size": "int32",
    "totalItems": "int32",
    "totalPages": "int32"
}
```

### Properties

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|items[] 	|array| List of user		|Optional| 
|items[].authorizedCustomers[]| 	array|List of authorized customer|	Optional|
|items[].authorizedCustomers[].customerId| 	string|Customer id|	Optional|
|items[].authorizedCustomers[].name| 	string|Customer name|	Optional|
|items[].createdAt| 	date-time|Creation date time|	Optional, read only. |
|items[].email| 	string|Email||
|items[].emails[]| 	array|List of emails sent to the user|Optional|
|items[].emails[].createDate| 	date-time|Create date time|	Optional, read only.|
|items[].emails[].emailId 	|string|Unique identifier of the email in the mail storage|Optional, read only.|
|items[].emails[].error| 	string|Error message if the email is not send|	Optional, read only.|
|items[].emails[].id| 	string|Unique identifier|	Optional, read only.|
|items[].emails[].type| 	string 	|Type|	Optional, read only. |
|items[].firstName| 	string|First name||	
|items[].id| 	string|Unique identifier|	Optional, read only. |
|items[].lastName| 	string|Last name||
|items[].password| 	string|Password||
|items[].roles[]| 	array|List of roles|Optional|
|items[].roles[].clientRole| 	boolean|| 		Optional|
|items[].roles[].composite| 	boolean|| 		Optional|
|items[].roles[].composites| 	object| 	Composites| 	Optional|
|items[].roles[].composites.application| 	object| 		|Optional|
|items[].roles[].composites.application.<key\>[]| 	array of string|| 		Optional|
|items[].roles[].composites.client| 	object|| 		Optional|
|items[].roles[].composites.client.<key\>[]| 	array of string|| 		Optional|
|items[].roles[].composites.realm[]| 	array of string|| 		Optional|
|items[].roles[].containerId| 	string|| 		Optional|
|items[].roles[].description| 	string|| 		Optional|
|items[].roles[].id| 	string|| 		Optional|
|items[].roles[].name| 	string|| 		Optional|
|items[].roles[].scopeParamRequired| 	boolean|| 		Optional|
|items[].status| 	string|Status, possible values are: `ENABLED`,`DISABLED`|	Optional|
|items[].token| 	string|Token used for reset the password|	Optional, read only.|
|items[].userName| 	string|Name||
|last| 	boolean|Last page or not|	Read only.|
|number| 	int32|Current page number|	Read only.|
|size|	int32|Number of carts per page|	Read only.|
|totalItems| 	int64|Total number of carts|	Read only.|
|totalPages| 	int32|Total number of pages|	Read only.|
