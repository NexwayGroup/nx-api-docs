
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

:::note
Order related notifications listed above don't include subscription details.
:::

The following notifications include details about the subscription. Although, subscriptions are a separate domain with their own [set of events](10-subscriptionNotification.md).:

* **Order Completed With Content Including Subscription Data**
`type=completed`
* **Order Canceled With Content Including Subscription Data**
`type=canceled`


## List of fields 

<!-- GEN:notification:OrderNotification subject="`order`" type="Event type — see [Event list](#event-list)" objectId="Order identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `order` | R |
| type | string | Event type — see [Event list](#event-list) | R |
| objectId | string | Order identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| order | order.Order |  | R |
| → cancellationDate | string (date-time) | Cancellation timestamp in ISO 8601 format | O |
| → cancellationReason | string | Reason for order cancellation | O |
| → cartId | string | Associated cart identifier | O |
| → consentFlags | order.ConsentFlags |  | R |
| — → newsletterOptin | boolean | Marketing newsletter consent indicator | R |
| → creationDate | string (date-time) | Order creation date in ISO 8601 format | R |
| → currency | string | Currency code, e.g. EUR, USD | R |
| → customer | order.Customer |  | R |
| — → customerAccount | string | Realm/account name | R |
| — → id | string | Customer identifier | R |
| — → name | string |  | O |
| — → serveur | string |  | O |
| → decodedExternalContext | object | Decoded cart parameters | O |
| → externalContext | string | Base64 encoded string of cart parameters | O |
| → id | string | Order identifier | R |
| → items | order.LineItem[] | Order line items | R |
| — → activationCode | string | Product activation code | O |
| — → discountPlan | order.DiscountPlan |  | O |
| — — → discountStep | integer (int32) | Step in the discount plan | R |
| — — → ignorePurchaseDiscount | boolean | Whether to ignore discount on the initial purchase | O |
| — — → tag | string | Plan designation tag | R |
| — → discountRate | number (double) | Discount rate applied | O |
| — → expirationDate | string (date-time) | Product expiration timestamp in ISO 8601 format | O |
| — → externalContext | string | External context data | R |
| — → fulfillmentId | string | Fulfillment process identifier | O |
| — → id | string | Unique line item identifier | R |
| — → isUpsell | boolean | Upsell status marker | O |
| — → lineItemId | string | Line item identifier | R |
| — → pricing | order.Pricing |  | R |
| — — → allDiscountsApplied | com.nx.notification.externalapi.DiscountApplied[] | All discounts applied to this item | O |
| — — — → discountAmount | number (double) | Discount value | R |
| — — — → discountCode | string | Customer-facing discount code | O |
| — — — → discountId | string | Monetize discount identifier | R |
| — — — → discountRate | number (double) | Discount percentage, e.g. 0.2 for 20% | O |
| — — → cumulatedDiscountRate | number (double) | Total cumulated discount percentage | O |
| — — → unitPriceExclVAT | number (double) | Pre-tax unit cost | R |
| — — → unitPriceIncVAT | number (double) | Post-tax unit cost | R |
| — — → vatAmount | number (double) | Tax amount | O |
| — — → VATRate | number (double) | Tax rate percentage | R |
| — → priority | integer (int32) | Line item priority | O |
| — → product | order.Product |  | R |
| — — → name | string | Product name | R |
| — — → publisherReference | string | Publisher's internal product identifier | O |
| — — → uniqueReference | string | Unique product identifier on the Nexway Monetize platform | R |
| — → publisherFulfillmentId | string | Publisher fulfillment identifier | O |
| — → quantity | integer (int32) | Product quantity | R |
| — → subItems | order.SubItem[] | Nested line items | O |
| — — → activationCode | string |  | O |
| — — → externalContext | string |  | O |
| — — → fulfillmentId | string |  | O |
| — — → id | string |  | O |
| — — → product | order.Product |  | O |
| — — — → name | string | Product name | R |
| — — — → publisherReference | string | Publisher's internal product identifier | O |
| — — — → uniqueReference | string | Unique product identifier on the Nexway Monetize platform | R |
| — — → publisherFulfillmentId | string |  | O |
| — → subscription | subscription.Subscription |  | O |
| — — → createDate | string (date-time) | Creation date in ISO 8601 format | R |
| — — → discountPlan | subscription.DiscountPlan |  | O |
| — — — → nextDiscountStep | integer (int32) | Calculated next discount step value | R |
| — — — → startingStep | integer (int32) | Starting step | O |
| — — — → tag | string | Plan identifier | R |
| — — → enduserId | string | End-user UUID | R |
| — — → id | string | Subscription UUID | R |
| — — → lifecycle | subscription.LifeCycle |  | R |
| — — — → anniversaryDate | string (date-time) | Renewal/expiration date in ISO 8601 format | R |
| — — — → generation | integer (int32) | Renewal generation (term number) | R |
| — — — → id | string | Back-office subscription identifier (deprecated) | O |
| — — — → status | string | Status: Active, Suspended, Dunning, Canceled, Expired | R |
| — — → modelId | string | Model identifier, e.g. NEXWAY_1Y | R |
| — — → name | string | Product name | O |
| — — → paymentMethodIsNoLongerReplayable | boolean | Whether the payment method is no longer replayable | O |
| — — → paymentMethodIsNoLongerReplayableReason | string | Reason why the payment method is no longer replayable | O |
| — — → products | subscription.Product[] | Associated products | R |
| — — — → id | string | Product UUID | R |
| — — — → licenseId | string | License identifier | O |
| — — — → lineItemId | string | Order line item UUID | R |
| — — → source | string | Subscription source | O |
| — — → storeId | string | Store UUID | R |
| — → subscriptionId | string | Associated subscription identifier | O |
| — → trial | boolean | Trial status indicator | R |
| — → trialDuration | integer (int32) | Trial duration in days | O |
| — → unitPriceExclVAT | number (double) | Unit price excluding tax | R |
| — → unitPriceIncVAT | number (double) | Unit price including tax | R |
| — → VATRate | number (double) | Applied sales tax rate | R |
| → offerSubSource | string | Sub-source designation, e.g. SUSPEND, RESUME | O |
| → payment | order.Payment |  | O |
| — → amount | number (double) | Payment amount | O |
| — → automaticBilling | boolean | Whether the payment was automatically charged | R |
| — → externalContext | string | External context data | O |
| — → id | string | Payment identifier | R |
| — → lastError | order.LastPaymentError |  | O |
| — — → code | string | Error code | R |
| — — → message | string | Error description | O |
| — → method | string | Payment method, e.g. visa, mastercard, sepa, pix, boleto | O |
| — → silentOnFailure | boolean | Whether to suppress failure notifications | O |
| — → status | string | Payment status: COMPLETED, FAILED | R |
| — → transitionPaymentDate | string (date-time) | Transaction timestamp in ISO 8601 format | R |
| → payments | order.Payment[] | All payment attempts | O |
| — → amount | number (double) | Payment amount | O |
| — → automaticBilling | boolean | Whether the payment was automatically charged | R |
| — → externalContext | string | External context data | O |
| — → id | string | Payment identifier | R |
| — → lastError | order.LastPaymentError |  | O |
| — — → code | string | Error code | R |
| — — → message | string | Error description | O |
| — → method | string | Payment method, e.g. visa, mastercard, sepa, pix, boleto | O |
| — → silentOnFailure | boolean | Whether to suppress failure notifications | O |
| — → status | string | Payment status: COMPLETED, FAILED | R |
| — → transitionPaymentDate | string (date-time) | Transaction timestamp in ISO 8601 format | R |
| → refund | order.Refund |  | O |
| — → lineItems | order.RefundDetails[] |  | O |
| — — → initialAmount | number (double) |  | O |
| — — → productId | string |  | O |
| — — → productName | string |  | O |
| — — → ratio | number (double) |  | O |
| — — → refundAmount | number (double) |  | O |
| — — → refundMethod | string |  | O |
| — → totalAmountRefunded | number (double) |  | O |
| → salesFlag | string[] | Sales flags | O |
| → source | string | Order origin: PURCHASE, SUBSCRIPTION, OFFER, MANUAL_RENEWAL | R |
| → status | string | Current order status | R |
| → store | order.Store |  | R |
| — → id | string | Store identifier | R |
| → totalPriceExclVAT | number (double) | Total price excluding sales tax | R |
| → totalPriceIncVAT | number (double) | Total price including sales tax | R |
| → user | order.EndUser |  | R |
| — → city | string | City | O |
| — → companyName | string | Company name | O |
| — → companyRegistrationNumber | string | Company registration number | O |
| — → country | string | Alpha-3 country code | R |
| — → email | string | Buyer email address | R |
| — → firstName | string | First name | O |
| — → id | string | Buyer identifier | R |
| — → language | string | Alpha-2 language code | O |
| — → lastName | string | Last name | O |
| — → region | string | Region | O |
| — → street | string | Street address | O |
| — → zipcode | string | Postal code | O |
<!-- /GEN:notification:OrderNotification -->

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

