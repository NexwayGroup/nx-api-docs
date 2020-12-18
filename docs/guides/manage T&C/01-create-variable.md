
## Create a template variable
### Request

`POST https://api.nexway.store/tandcs/variables`

### Request body

The request body takes a complete [TemplateVariable resource](/docs/guides/manage%20T&C/17-templateVariable.md), containing the following writable properties:

```json
{
    "cancelPeriod": "string",
    "cancelPeriodI18nConfiguration": {
        "<key>": {
            "<key>": "string"
        }
    },
    "contactForm": "string",
    "corporateUrl": "string",
    "createDate": "date-time",
    "dbVersion": "int64",
    "id": "string",
    "nexwaySasCapital": "string",
    "startDate": "date-time",
    "updateDate": "date-time"
}
```

### Properties

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|cancelPeriod| 	string| 		|Optional|
|cancelPeriodI18nConfiguration| 	object|| 		Optional|
|cancelPeriodI18nConfiguration.<key\>| 	map of object|| 		Optional|
|cancelPeriodI18nConfiguration.<key\>.<key\>| 	map of string| 		|Optional|
|contactForm| 	string|| 		Optional|
|corporateUrl| 	string|| 		Optional|
|createDate| 	date-time|| 		Optional|
|dbVersion| 	int64|| 		Optional|
|id| 	string|| 		Optional|
|nexwaySasCapital| 	string|| 		Optional|
|startDate| 	date-time| 		|Optional|
|updateDate| 	date-time|| 		Optional| 

### Authorisation

This request requires the use of one of following authorisation methods: `OAuth2`.

### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|201| 	**Created** Success ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||
