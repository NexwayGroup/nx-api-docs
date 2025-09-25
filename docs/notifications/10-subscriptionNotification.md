# Subscription Notifications
You can receive notifications whenever your subscription status changes.

## Event list
* created
* suspended
* reactivated
* canceled
* subscription discount plan is changed

## List of fields

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | subscription unique identifier | R |
| modelId | subscription model id | R |
| name | subscription product name | O |
| lifecycle |  | R |
| lifecycle.id | internal id | O |
| lifecycle.anniversaryDate | Subscription anniversary (expiration) date in ISO 8601 format | R |
| lifecycle.generation | Subscription generation | R |
| lifecycle.status | Subscription status  | R |
| lifecycle.endUserId | Id of end-user, connected to subscription  | R |
| products[] | List of products in subscription | R |
| products[].id | product id | R |
| products[].lineItemId | original order line item id | R |

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
| ---- | ----------- | --- |
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
This notification allows to be notified if end-user changes payment method for subscription

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | Subscription unique identifier | R |
| paymentMethod | New payment method | O |
| expirationDate | New payment method expiration date | O |

## Subscription enters prebilling period
This notification allows to track when is subscription reached of prebilling period

| Name | Description | R/O |
| ---- | ----------- | --- |
| subscriptionId | Subscription unique identifier | R |
| subscriptionStatus; | Subscription status | R |
