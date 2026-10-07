# End-user (Shopper) Notifications

These notifications are triggered when the End-user data changes. 

## Event list

* **End User Created**  
* **End User Updated**  
* **EndUser Deleted** 


## EndUser Created/Updated Payload Structure

<!-- GEN:notification:EndUserNotification subject="endUser" type="created|updated" objectId="EndUser identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | endUser | R |
| type | string | created|updated | R |
| objectId | string | EndUser identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| endUser | enduser.EndUserDetails |  | O |
| → city | string | City | O |
| → country | string | Alpha-2 country code | R |
| → customerId | string | Customer identifier the end user belongs to | R |
| → email | string | End user email address | R |
| → firstName | string | First name | O |
| → id | string | End user identifier | R |
| → lastName | string | Last name | O |
| → locale | string | Locale used by the end user, e.g. `fr-FR` | R |
| → region | string | Region code, e.g. `FR-MQ` | O |
| → storeId | string | Store identifier the end user belongs to | R |
<!-- /GEN:notification:EndUserNotification -->

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

<!-- GEN:notification:PaymentMethodAddedNotification subject="endUser" type="paymentMethodAddedToWallet" objectId="EndUser identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | endUser | R |
| type | string | paymentMethodAddedToWallet | R |
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