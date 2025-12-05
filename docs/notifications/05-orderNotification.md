
# Order Notifications

You can receive notifications whenever your order status changes.
See the details of the [order processing here](../guides/40-order-processing.md).

## Event list

Below is a list of order-related notifications which you can subscribe to:

* **Order Created**
`type=created`
* **Order Payment Failed**
`type=paymentFailed`
Usually caused by an internal technical issue
* **Order Payment Refused**
`type=paymentRefused`
By a payment gateway or bank
* **Order Completed**
`type=completed`
* **Order Completed With Error**
`type=partiallyCompleted`
* **Order Fulfillment Failed**
`type=fulfillmentFailed`
* **Order Cancelled**
`type=canceled`
* **Order Aborted**
`type=aborted`
* **Subscription Order Renew Completed**
`type=renewCompleted`
* **Prebilling Order Created**
`type=prebillingStarted`
* **Offer Prebilling Started**
`type=prebillingStarted`
Same as Prebilling Order Created, but triggered if a prebilling order with source `OFFER` already exists

## List of fields 

The notification payload will include an 'order' object containing the following details:

| Name | Description | R/O |
| ---- | ----------- | --- |
| **subject** | For order notifications the subject is `order` | R |
| **type** | Depends on the type of the notification | R |
| **objectId** | Order unique identifier | R |
| **eventDate** | date in ISO 8601 format | R |
| **order** `{object}`| Order object | R |
| **order.id** | Order unique identifier | R |
| **order.status** | Order status corresponds to the event type. | R |
| **order.cancellationReason** | Will be provided only if order is canceled event is sent AND reason was set for cancelation | O |
| **order.cancellationDate** | | O |
| **order.source** | Order source: PURCHASE, SUBSCRIPTION, OFFER, MANUAL_RENEWAL, etc | R |
| **order.offerSubSource** | Order sub source: SUSPEND, RESUME | O |
| **order.creationDate** | Creation date in ISO 8601 format, ex.: 2024-01-01T01:02:03Z | R |
| **order.currency** | Order's Currency, ex.: EUR | R |
| **order.salesFlag** `[string]` | Sales flags is an array of strings provided in the cart. Similar to external context, but unencoded. | O |
| **order.totalPriceIncVAT** | Order total price, including sales tax | R |
| **order.totalPriceExclVAT** | Order total price, excluding sales tax | R |
| **order.externalContext** | Based64 encoded string of cart parameters | O |
| **order.decodedExternalContext** | Decoded map of cart string parameters if they were provided in the json format | O |
| **order.user** `{object}` | Buyer's details object | R |
| **order.user.id** | Buyer's id | O |
| **order.user.email** | Buyer's email | R |
| **order.user.firstName** | Buyer's first name | O |
| **order.user.lastName** | Buyer's last name | O |
| **order.user.language** | Buyer's language alpha-2 code, ex "pt" | O |
| **order.user.country** | Buyer's country alpha-3 code, ex "BRA" | R |
| **order.user.city** | Buyer's city | O |
| **order.user.region** | Buyer's region | O |
| **order.user.street** | Buyer's street address | O |
| **order.user.zipcode** | Buyer's postal code | O |
| **order.items** `[{object}]` | List of items purchased (products, services, etc.) | R |
| **order.items[].id** | Unique ID for order line item | R |
| **order.items[].product** `{object}`| Product object | R |
| **order.items[].product.name** | Product name | R |
| **order.items[].product.uniqueReference** | A unique ID for identifying your product on the Nexway Monetize platform | R |
| **order.items[].product.publisherReference** | A unique ID for identifying your product in your information system, if defined | O |
| **order.items[].fulfillmentId** | Fulfillment process identifier | O |
| **order.items[].quantity** | Product quantity | R |
| **order.items[].activationCode** | Product activation code | O |
| **order.items[].expirationDate** | Expiration date of a product (if any) | O |
| **order.items[].pricing** `{object}` | Price object | R |
| **order.items[].pricing.unitPriceExclVAT** | Product unit price, excluding sales tax | R |
| **order.items[].pricing.unitPriceIncVAT** | Product unit price, including sales tax | R |
| **order.items[].pricing.vatAmount** | Vat amount | O |
| **order.items[].pricing.cumulatedDiscountRate** | Discount rate applied to product | O |
| **order.items[].pricing.allDiscountsApplied** `[{object}]`| Array of all applied discounts. Each discount represneted as object | O |
| **order.items[].pricing.allDiscountsApplied[].discountRate**| Discount rate, f.e. 0.2 | O |
| **order.items[].pricing.allDiscountsApplied[].discountId**| Identifier of discount in Monetize | R |
| **order.items[].pricing.allDiscountsApplied[].discountCode**| Discount code (if any) | O |
| **order.items[].pricing.allDiscountsApplied[].discountAmount**| Amount of discount | O |
| **order.items[].pricing.VATRate** | Sold product applied sales tax rate | R |
| **order.items[].unitPriceExclVAT** | Product unit price, excluding sales tax | R |
| **order.items[].unitPriceIncVAT** | Product unit price, including sales tax | R |
| **order.items[].trial** | | R |
| **order.items[].subscriptionId** | SubscriptionId if the line item has one | O |
| **order.items[].activationCode** | Product activation code | O |
| **order.items[].subItems []** | | R |
| **order.items[].isUpsell** | Boolean which marks a product as upsold or not | R |
| **order.items[].VATRate** | Sold product applied sales tax rate | R |
| **order.items[].DiscountPlan** `{object}` | Subscription discount plan node will exist in case if discount plan is applied to subscription | O |
| **order.items[].DiscountPlan.tag** | Subscription discount plan tag | R |
| **order.items[].DiscountPlan.discountStep** | Subscription discount plan step is used on acquisition | O |
| **order.items[].DiscountPlan.ignorePurchaseDiscount**| Only in case if this flag was set on acquisition | O |
| **order.customer** `{object}` | Customer object | R |
| **customer.id** | customer id connected to the order | R |
| **order.customer.customerAccount** | customer realm name  connected to the order | R |
| **order.customer.serveur** | customer  name  connected to the order | R |
| **order.store** `{object}` | Store object | R |
| **order.store.Id** | store id from an order | R |
| **order.cartId** | cartId from an order | O |
| **order.payment** `{object}` | Payment object (last) | O |
| **order.payment.id** | Payment id | R |
| **order.payment.method** | Payment method id (visa, mastercard, sepa, visa_electron, visa_inst4, diners, pix, boleto etc.) | O |
| **order.payment.amount** | | O |
| **order.payment.status** | Payment status (COMPLETED, FAILED) | R |
| **order.payment.transitionPaymentDate** | Transaction date, equal to transaction creation date | R |
| **order.payment.automaticBilling** | Boolean which highlights if payment was automatically charged or not | R |
| **order.payment.lastError** `{object}` | Last error object | O |
| **order.payment.lastError.code** | Error code | O |
| **order.payment.lastError.message** | Error message | O |
| **order.payments[]** | | O |
| **order.payments[].id** | | R |
| **order.payments[].method** | | O |
| **order.payments[].amount** | | O |
| **order.payments[].status** | | R |
| **order.payments[].transitionPaymentDate** | | R |
| **order.ConsentFlags** `{object}` | Consent flags object | R |
| **order.ConsentFlags.newstellerOption** | Marketing newsletter consent flag | R |

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

## Additional order events with subscription details

These are the same events as main order events listed above, but with additional information, which may be included if the order has subscription information:

* Order Completed With Content Including Subscription Data
* Order Canceled With Content Including Subscription Data

### Additional fields for 'Order Completed With Content Including Subscription Data' notification

Certain notification types, like 'Order Completed with Subscription Data' and 'Order Cancelled with Subscription Data' include details about the subscription. Although, subscriptions are a separate domain with their own [set of events](10-subscriptionNotification.md).

| Name | Description | R/O |
| ---- | ----------- | --- |
| **order.items[].subscription** | Subscription object | O |
| **order.items[].subscription.id** | Subscription identifier | R |
| **order.items[].subscription.createDate** | The date when subscription is created | R |
| **order.items[].subscription.modelId** | Internal subscription model identifier | R |
| **order.items[].subscription.name** | The name of subscription. Usually the same as the product name | R |
| **order.items[].subscription.storeId** | Internal store identifier | R |
| **order.items[].subscription.lifecycle** `{object}` | Lifecycle object | R |
| **order.items[].subscription.lifecycle.id** | Nexway back-office subscription identifier | O |
| **order.items[].subscription.lifecycle.anniversaryDate**| | R |
| **order.items[].subscription.lifecycle.generation**| F.e. after a purchase the generation is 0, after first renewal its 1 and so on | O |
| **order.items[].subscription.lifecycle.status** | Internal status | O |
| **order.items[].subscription.products** `[{object}]` | Array of Products  | R |
| **order.items[].subscription.products[].id** | Product identifier | R |
| **order.items[].subscription.products[].lineItemId** | | R |
| **order.items[].subscription.discountPlan** `{object}` | Subscription discount plan node will exist in case if discount plan is applied to subscription | O |
| **order.items[].subscription.discountPlan.tag** | Subscription discount plan tag | O |
| **order.items[].subscription.discountPlan.startingStep**| | O |
| **order.items[].subscription.enduserId** | Internal id of end-user | R |
