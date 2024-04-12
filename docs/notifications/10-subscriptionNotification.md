# Subscription Notifications
You can receive notifications whenever your subscription status changes.

## Event list
* created
* suspended
* reactivated
* canceled

## List of fields

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | subscription unique identifier | R |
| modelId | subscription model id | R |
| name | subscription product name | O |
| lifecycle |  | R |
| lifecycle.id | internal id | O |
| lifecycle.anniversaryDate | Subscription anniversary (expiration) date in ISO 8601 format | R |
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
