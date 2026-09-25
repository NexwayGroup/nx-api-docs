# End-user (Shopper) Notifications

These notifications are triggered when the End-user data changes. 

## Event list

* **EndUser Deleted**  
when an end-user account is deleted

## EndUser Deleted Payload Structure

| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `endUser` | R |
| type | string | `deleted` | R |
| objectId | string | EndUser identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |


## Wallet Event list

These events are fired when some payment is added or deleted from the wallet. There are separate events for credit cards, SEPA and all other uncategorized recurring payment methods. However, for the payment method deletion there is a single event.

* **Credit Card Added**
* **SEPA Payment Method Added**
* **Recurring Payment Method Added**
* **Payment Method Deleted**

## Payment Method Event Payload Structure

<!-- GEN:notification:PaymentMethodAddedNotification subject="endUser" type="paymentMethodAddedToWallet / paymentMethodDeletedFromWallet" objectId="EndUser identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | endUser | R |
| type | string | subscriptionPaymentMethodUpdated | R |
| objectId | string | EndUser identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| addedPaymentMethods | enduser.PaymentMethod[] | List of added payment instruments | R |
| → type | string | Specific variant, e.g. visa, mastercard | R |
| → _id | string | Unique payment method identifier | R |
| → expirationDate | string | Expiration date | O |
| → id | string | Payment method identifier | R |
| → paymentMethodType | string | Broad classification, e.g. CreditCard | R |
| → status | string | Activation state, e.g. ACTIVATED | R |
<!-- /GEN:notification:PaymentMethodAddedNotification -->