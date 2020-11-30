# Create notification rules

## Request
```json
POST "https://api.nexway.store/customer-notifier/rules"
```

## Request body
The request body takes a complete [Rule Resource](./notification-rules.md) , containing the following writable properties:

```json
{
    "customerId": "string",
    "rules": [
        {
            "emailChannel": {
                "recipients": [
                    "string"
                ]
            },
            "eventType": "string",
            "isActive": "boolean",
            "resource": "string",
            "webHooks": {
                "urlList": [
                    {
                        "Host": "string",
                        "Path": "string",
                        "Scheme": "string"
                    }
                ]
            }
        }
    ]
}
```
## Properties

|Name|Type|Description|Additional|
|--- |--- |--- |--- |
|customerId|string|Id of the customer||
|rules[]|array|List||
|rules[].emailChannel|object||Optional|
|rules[].emailChannel.recipients[]|array of string|List of emails to send notifications|Optional|
|rules[].eventType|string|Event to be notified Possible values are :           `created`, `paymentInProgress`, `confirmed`, `fraudInProgress`, `subscriptionInProgress`, `fulfillmentInProgress`, `completed`, `partialCompleted`, `cancelledpartial`, `Cancelled`,`invoiceCreated`, `invoiceFailed`, `emailConfirmationSent` ||
|rules[].isActive|boolean|Set of rules activated or not|Optional|
|rules[].resource|string|Resource to be notified Subscription||
|rules[].webHooks|object||Optional|
|rules[].webHooks.urlList[]|array|List of urls to send notifications|Optional|
|rules[].webHooks.urlList[].Host|string|Hostname of the url|Optional|
|rules[].webHooks.urlList[].Path|string|Path of the url or uri|Optional|
|rules[].webHooks.urlList[].Scheme|string|Scheme of the url|Optional|




|Status code|Description|Resource|
|--- |--- |--- |
|`201`|CreatedSuccess|`success`|
|`400`|Bad RequestBad Request||
|`401`|UnauthorizedUnauthorized||
|`403`|ForbiddenForbidden||
|`409`|ConflictConflict||
|`500`|Internal Server ErrorFailure||
|`503`|Service UnavailableService Unavailable||



