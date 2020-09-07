# Subscription Notifications

## Event list
* created
* suspended
* reactivated
* canceled

## List of fields

| Name | Description |
| ---- | ----------- |
| id | subscription unique identifier |
| modelId | subscription model id |
| name | subscription product name |
| lifecycle | internal id |
| products[] | List of products in subscription |
| products[].id | product id |
| products[].lineItemId | original order line item id |

### Example
```json
{
   "subject":"subscription",
   "type":"canceled",
   "objectId":"c0a47254-fb78-4859-8954-d98ff5fb7730",
   "eventDate":"2020-09-07T13:46:57Z",
   "subscription":{
      "id":"c0a47254-fb78-4859-8954-d98ff5fb7730",
      "createDate":"1595918226381",
      "modelId":"NEXWAY_1M",
      "name":"Nexway Secure Connection",
      "lifecycle":{
         "id":"6862801"
      },
      "products":[
         {
            "id":"d4b35678-94ec-4e8c-acd5-d758a71ede7f",
            "lineItemId":"c5ad58a0-6f41-47cf-9ecc-ab57b034c25e"
         }
      ]
   }
}
```
