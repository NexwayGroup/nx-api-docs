
# Order Notifications

You can receive notifications whenever your order status changes.
See the details of the [order processing here](../guides/40-order-processing.md).

## Event list

Below is a list of order-related notifications which you can subscribe to:

* Order created
* Order payment failed (usually internal technical issue)
* Order payment refused (by payment gateway)
* Order completed
* Order completed with error
* Order fulfillment failed
* Order cancelled

## List of fields

The notification payload will include an 'order' object containing the following details:

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | Order unique identifier | R |
| status | Order status corresponds to the event type. | R |
| source | Order source: PURCHASE, SUBSCRIPTION, OFFER, MANUAL_RENEWAL, etc | R |
| creationDate | Creation date in ISO 8601 format, ex.: 2024-01-01T01:02:03Z | R |
| currency | Order's Currency, ex.: EUR | R |
| totalPriceIncVAT | Order total price, including sales tax | R |
| totalPriceExclVAT | Order total price, excluding sales tax | R |
| salesFlag | Sales flags is an array of strings provided in the cart. Similar to external context, but unencoded. | O |
| consentFlags | Consent flags given by the end user | O |
| consentFlags.newsletterOptin | Marketing newsletter consent flag | O |
| externalContext | Based64 encoded string of cart parameters | O |
| decodedExternalContext | Decoded map of cart string parameters if they were provided in the json format | O |
| payment | Payment object | O |
| payment.id | Payment id | R |
| payment.method | Payment method id (visa, mastercard, sepa, visa_electron, visa_inst4, diners, pix, boleto etc.) | O |
| payment.status | Payment status (COMPLETED, FAILED) | R |
| user | Buyer's details object | O |
| user.id | Buyer's id | O |
| user.email | Buyer's email | R |
| user.firstName | Buyer's first name | O |
| user.lastName | Buyer's last name | O |
| user.language | Buyer's language alpha-2 code, ex "pt" | O |
| user.country | Buyer's country alpha-3 code, ex "BRA" | O |
| user.street | Buyer's street address | O |
| user.zipcode | Buyer's postal code | O |
| user.city | Buyer's city | O |
| items[] | List of items purchased (products, services, etc.) | R |
| items[].id | Unique ID for order line item | R |
| items[].product.name | Product name | R |
| items[].product.uniqueReference | A unique ID for identifying your product on the Nexway Monetize platform | R |
| items[].product.publisherReference | A unique ID for identifying your product in your information system, if defined | O |
| items[].fulfillmentId | Fulfillment process identifier | O |
| items[].quantity | Product quantity | R |
| items[].activationCode | Product activation code | O |
| items[].unitPriceIncVAT | Product unit price, including sales tax | R |
| items[].unitPriceExclVAT | Product unit price, excluding sales tax | R |
| items[].VATRate | Sold product applied sales tax rate | R |
| items[].isUpsell | Boolean which marks a product as upsold or not | R |
| items[].discountRate | Discount rate applied to product | O |
| items[].subscriptionId | SubscriptionId if the line item has one | O |
| items[].discountPlan | Subscription discount plan node will exist in case if discount plan is applied to subscription | O |
| items[].discountPlan.tag | Subscription discount plan tag | R |
| items[].discountPlan.discountStep | Subscription discount plan step is used on acquisition | R |
| items[].discountPlan.ignorePurchaseDiscount | Only in case if this flag was set on acquisition | O |

If you need to get additional data, which is not available in the event, please refer to the REST API methods to get order or other entities.

### Example

```json
{
  "subject" : "order",
  "type" : "completed",
  "objectId" : "42WRNTVCTVJ",
  "eventDate" : "2025-02-07T07:00:20Z",
  "order" : {
    "id" : "42WRNTVCTVJ",
    "status" : "COMPLETED",
    "source" : "PURCHASE",
    "creationDate" : "2025-02-07T07:00:00Z",
    "payment" : {
      "id" : "42WRNTVCTVJ0",
      "method" : "visa",
      "amount" : 55.0,
      "status" : "COMPLETED",
      "transitionPaymentDate" : "2025-02-07T07:00:08Z",
      "automaticBilling" : false
    },
    "payments" : [ {
      "id" : "42WRNTVCTVJ0",
      "method" : "visa",
      "amount" : 55.0,
      "status" : "COMPLETED",
      "transitionPaymentDate" : "2025-02-07T07:00:08Z",
      "automaticBilling" : false
    } ],
    "currency" : "AUD",
    "totalPriceIncVAT" : 55.0,
    "totalPriceExclVAT" : 50.0,
    "externalContext" : "e30=",
    "decodedExternalContext" : { },
    "salesFlag" : [ ],
    "consentFlags" : {
      "newsletterOptin" : false
    },
    "user" : {
      "id" : "b8dccf29-6f3b-4551-8ebf-98d3ef47f40a",
      "email" : "billyjoe@nexway.com",
      "firstName" : "Billy",
      "lastName" : "Joe",
      "language" : "en",
      "country" : "AUS",
      "zipcode" : "3249",
      "city" : "Gerangamete"
    },
    "items" : [ {
      "id" : "f4fc4d98-30f1-427e-9953-6a6966654d71",
      "product" : {
        "name" : "Acme Standard",
        "uniqueReference" : "a7c55bec-b1b1-401e-b6cb-d6121ca1f66b",
        "publisherReference" : "ACME_XYZ"
      },
      "quantity" : 1,
      "expirationDate" : "2026-05-07T06:59:54Z",
      "unitPriceExclVAT" : 50.0,
      "unitPriceIncVAT" : 55.0,
      "trial" : false,
      "trialDuration" : 7,
      "subscriptionId" : "c37570f9-ebc3-4817-8da6-a77339224739",
      "fulfillmentId" : "6c0edf3e-b73e-46b7-944b-f424708d5b2f",
      "activationCode" : "XXXX-YYYY-ZZZZ-JSJSK",
      "subscription" : {
        "id" : "c37570f9-ebc3-4817-8da6-a77339224739",
        "createDate" : "2025-02-07T07:00:10Z",
        "modelId" : "NEXWAY_1Y",
        "name" : "Acme Standard",
        "storeId" : "59409482-9719-4d76-97ad-c679acc7d14a",
        "lifecycle" : {
          "id" : "9141850",
          "anniversaryDate" : "2026-05-07T06:59:54Z"
        },
        "products" : [ {
          "id" : "a7c55bec-b1b1-401e-b6cb-d6121ca1f66b",
          "lineItemId" : "93c9b6a3-e33e-4b88-bb1b-75134f23e298"
        } ]
      },
      "subItems" : [ ],
      "discountPlan" : {
        "tag" : "newtyptag",
        "discountStep" : 0,
        "ignorePurchaseDiscount" : false
      },
      "VATRate" : 0.1,
      "isUpsell" : true
    } ]
  }
}
```

### Additional fields for 'Order Completed With Content Including Subscription Data' notification

Certain notification types, like 'Order Completed with Subscription Data' and 'Order Cancelled with Subscription Data' include details about the subscription. Although, subscriptions are a separate domain with their own [set of events](10-subscriptionNotification.md).

| Name | Description | R/O |
| ---- | ----------- | --- |
| items[].subscription | Subscription object | O |
| items[].subscription.id | SubscriptionId | R |
| items[].subscription.createDate | Creation date in ISO 8601 format | R |
| items[].subscription.modelId | Subscription model | R |
| items[].subscription.storeId | Selling store id | R |
| items[].subscription.lifecycle | Lifecycle data | R |
| items[].subscription.lifecycle.id | Lifecycle id string | O |
| items[].subscription.lifecycle.anniversaryDate | Subscription anniversary (expiration) date in ISO 8601 format | R |
