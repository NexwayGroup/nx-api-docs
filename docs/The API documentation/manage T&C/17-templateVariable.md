
## TemplateVariable resource
### Methods

- [post](/docs/The%20API%20Documentation/manage%20T&C/01-create-variable.md) - Create a template variable
- [get](/docs/The%20API%20Documentation/manage%20T&C/02-get-all-variables.md) - Get a list of template variables
- [put](/docs/The%20API%20Documentation/manage%20T&C/03-update-variable.md) - Update a template variable

### Resource

```json
[
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
]
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
