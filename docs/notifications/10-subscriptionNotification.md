# Subscription Notifications
You can receive notifications whenever your subscription status changes.

## Event list
* created
* suspended
* reactivated
* canceled
* renewed
* expired
* subscription discount plan is changed
* subscription expiration date is updated
* payment method is changed

## List of fields

| Name | Description | R / O |
| :--- | :--- | :---: |
| **subject** | subscription | R |
| **type** | Depending on event:<br>• expiredsubscription<br>• expirationdateupdated<br>• expiration date updated<br>• reactivated<br>• suspended<br>• canceled<br>etc | R |
| **objectId** | subscriptonId: `afe09ab2-25d0-4fdb-857e-dc22ff21ce8f` | R |
| **eventDate** | `2025-07-25T11:30:03Z` | |
| **subscription {}** | | R |
| **subscription.id** | `afe09ab2-25d0-4fdb-857e-dc22ff21ce8f` | R |
| **subscription.createDate** | `2025-07-25T11:30:03Z` | R |
| **subscription.modelId** | `NEXWAY_1Y` | R |
| **subscription.name** | `Kaspersky Endpoint Security Cloud` | O |
| **subscription.storeId** | `36f48867-d6ca-42d3-bf55-5f54a6740803` | R |
| **subscription.lifecycle {}** | | R |
| **subscription.lifecycle.id** | `9191920` | O |
| **subscription.lifecycle.generation** | 0 | R |
| **subscription.products {}** | | R |
| **subscription.products.id** | `ecaa7302-6686-4527-a034-26931397d7ca` | R |
| **subscription.products.lineItemId**| `b60928b9-523e-42b7-9780-2527e5bdcb1e` | R |
| **subscirption.lifecycle.anniversaryDate**| | R |
| **subscirption.lifecycle.status** | | R |
| **subscription.endUserId** | | R |

### Example
```json
{
   "subject": "subscription",
   "type": "created",
   "objectId": "c0a47254-fb78-4859-8954-d98ff5fb7730",
   "eventDate": "2020-09-07T13:46:57Z",
   "subscription": {
      "id": "c0a47254-fb78-4859-8954-d98ff5fb7730",
      "createDate":"1595918226381",
      "modelId": "NEXWAY_1M",
      "name": "Nexway Secure Connection",
      "lifecycle": {
         "id": "6862801",
         "anniversaryDate" : "2026-04-09T15:30:38Z"
      },
      "products":[
         {
            "id": "d4b35678-94ec-4e8c-acd5-d758a71ede7f",
            "lineItemId": "c5ad58a0-6f41-47cf-9ecc-ab57b034c25e"
         }
      ]
   }
}
```
## Subscription discount plan is changed
This is special notification which will be send in case if subscription plan is changed for subscription. The payload is following:

| Name | Description | R/O |
| :--- | :--- | :---: |
| subscription.id | Subscription unique identifier | R |
| subscription.discountPlan | Node with subscription plan is added / changed for a given subscription | R |
| subscription.discountPlan.tag | Subscription discount plan tag is added / chagned | R |
| subscription.discountPlan.nextDiscountStep | The number of next discount plan step will be used for subscription discounting. Calculates by formula nextDiscountStep=startingStep + generation + 1 | R |


### Example
```json
{
  "subject" : "subscription",
  "type" : "discount plan updated",
  "objectId" : "d888ff3b-0381-4b35-9cbe-f9c73666524f",
  "eventDate" : "2025-01-28T08:58:42Z",
  "subscription" : {
    "id" : "d888ff3b-0381-4b35-9cbe-f9c73666524f",
    "discountPlan": {
      "tag": "tag"
      "nextDiscountStep": 1
    }
  }
}
```

## Payment method is changed for subscription
This notification is sent when the end-user changes subscription payment method. 

| Name | Description | R / O |
| :--- | :--- | :---: |
| **subject** | `endUser` | R |
| **type** | `subscriptionPaymentMethodUpdated` | R |
| **objectId** | guide of an object (enduser in this case) | R |
| **eventDate** | `2025-09-29T07:34:06Z` | R |
| **enduser {}** | | R |
| **enduser.id** | payment method id, guide: `8d2eaaf6-9eba-4400-9927-516672d4693b` | R |
| **enduser.subscriptionId** | subscription id | R |
| **enduser.paymentMethod** | payment method type, f.e. creditCard | O |
| **enduser.expirationDate** | `11/2028` | O |

