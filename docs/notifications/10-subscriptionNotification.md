# Subscription Notifications
You can receive notifications whenever your subscription status changes.

## Event list

* **Subscription Created**
* **Subscription Suspended**
* **Subscription Reactivated**
* **Subscription Canceled**
* **Subscription Discount Plan Updated**
* **Subscription Expiration Date Updated** - triggered only when the date is shifted outside of the normal renewal process
* **Subscription Unpaid** - triggered with the email reminder about the unpaid subscription, which depends on the subscription model
* **Subscription expired** - sent when the grace period ends
* **Payment Method Changed**

## List of fields

<!-- GEN:notification:SubscriptionNotification subject="`subscription`" type="Event type — see [Event list](#event-list)" objectId="Subscription identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `subscription` | R |
| type | string | Event type — see [Event list](#event-list) | R |
| objectId | string | Subscription identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| subscription | subscription.Subscription |  | R |
| → createDate | string (date-time) | Creation date in ISO 8601 format | R |
| → discountPlan | subscription.DiscountPlan |  | O |
| — → nextDiscountStep | integer (int32) | Calculated next discount step value | R |
| — → startingStep | integer (int32) | Starting step | O |
| — → tag | string | Plan identifier | R |
| → enduserId | string | End-user UUID | R |
| → id | string | Subscription UUID | R |
| → lifecycle | subscription.LifeCycle |  | R |
| — → anniversaryDate | string (date-time) | Renewal/expiration date in ISO 8601 format | R |
| — → generation | integer (int32) | Renewal generation (term number) | R |
| — → id | string | Back-office subscription identifier (deprecated) | O |
| — → status | string | Status: Active, Suspended, Dunning, Canceled, Expired | R |
| → modelId | string | Model identifier, e.g. NEXWAY_1Y | R |
| → name | string | Product name | O |
| → paymentMethodIsNoLongerReplayable | boolean | Whether the payment method is no longer replayable | O |
| → paymentMethodIsNoLongerReplayableReason | string | Reason why the payment method is no longer replayable | O |
| → products | subscription.Product[] | Associated products | R |
| — → id | string | Product UUID | R |
| — → licenseId | string | License identifier | O |
| — → lineItemId | string | Order line item UUID | R |
| → source | string | Subscription source | O |
| → storeId | string | Store UUID | R |
<!-- /GEN:notification:SubscriptionNotification -->

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
This notification is sent when a discount plan is added or changed for a subscription.

| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `subscription` | R |
| type | string | `discountPlanUpdated` | R |
| objectId | string | Subscription UUID | R |
| eventDate | string (date-time) | Timestamp of the event in ISO 8601 format | R |
| subscription | object | | R |
| → id | string | Subscription UUID | R |
| → discountPlan | object | Discount plan added or changed for this subscription | R |
| — → tag | string | Discount plan tag | R |
| — → nextDiscountStep | integer | Next step value. Calculated as `startingStep + generation + 1` | R |


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


## Subscription Payment Method Changed
This notification is sent when the end-user changes subscription payment method.

<!-- GEN:notification:subscription.PaymentMethodUpdatedNotification subject="`endUser`" type="`subscriptionPaymentMethodUpdated`" objectId="EndUser identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `endUser` | R |
| type | string | `subscriptionPaymentMethodUpdated` | R |
| objectId | string | EndUser identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| endUser | subscription.EndUser |  | R |
| → expirationDate | string | The expiration date of the payment method mm/YY | O |
| → id | string | The end user id | R |
| → paymentMethod | string | Broad classification, e.g. CreditCard | O |
| → paymentMethodName | string | Specific variant, e.g. visa, mastercard | O |
| → subscriptionId | string | The subscription Id | R |
<!-- /GEN:notification:subscription.PaymentMethodUpdatedNotification -->

