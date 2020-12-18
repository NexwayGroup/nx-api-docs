
## Template resource
### Methods

- [post](/docs/guides/manage%20T&C/07-create-tand-c-template.md) - Create a template
- [get](/docs/guides/manage%20T&C/09-get-tand-c-template.md) - Get a template by id
- [put](/docs/guides/manage%20T&C/08-update-tand-c-template.md) - Update a template

### Resource

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