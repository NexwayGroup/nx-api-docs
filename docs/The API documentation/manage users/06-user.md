
## User resource

The User resource contains information about each one of a Nexway customer's colleagues or employees, and enables them to log on to Nexway Center and/or obtain a JWT to call directly secured APIs.

When users are created on the platform, they receive an email which contains a URL with a unique token that they must use to set their password.

### Methods

- [post](/docs/The%20API%20Documentation/manage%20users/01-create-user.md) - Create a user
- [get](/docs/The%20API%20Documentation/manage%20users/02-get-user.md) - Get a user by id

### Resource

```json
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
```

### Properties

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|authorizedCustomers[]| 	array|List of authorized customer|	Optional|
|authorizedCustomers[].customerId| 	string|Customer id|	Optional|
|authorizedCustomers[].name| 	string|Customer name|	Optional|
|createdAt| 	date-time|Creation date time|	Optional, read only. |
|email| 	string|Email||
|emails[]| 	array|List of emails sent to the user|Optional|
|emails[].createDate| 	date-time|Create date time|	Optional, read only.|
|emails[].emailId 	|string|Unique identifier of the email in the mail storage|Optional, read only.|
|emails[].error| 	string|Error message if the email is not send|	Optional, read only.|
|emails[].id| 	string|Unique identifier|	Optional, read only.|
|emails[].type| 	string 	|Type|	Optional, read only. |
|firstName| 	string|First name||	
|id| 	string|Unique identifier|	Optional, read only. |
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
|token| 	string|Token used for reset the password|	Optional, read only.|
|userName| 	string|Name||
