# End-user (Shopper) Notifications

These notifications are triggered when the End-user data changes. 

## Event list

* **EndUser Deleted**  
when an end-user account is deleted

## EndUser Deleted Payload Structure

<!-- GEN:basenotificationcontent -->
| Name | Description | R / O |
| :--- | :---------- | :---: |
| **subject** | Event entity type (`order`, `subscription`, `endUser`, etc.) | R |
| **type** | Notification event type (e.g. `completed`, `deleted`) | R |
| **objectId** | Entity identifier (orderId, subscriptionId, endUserId, etc.) | R |
| **eventDate** | ISO 8601 date of the event | R |
<!-- /GEN:basenotificationcontent -->

For this event: `subject` = `endUser`, `type` = `deleted`, `objectId` = EndUser identifier.


## Wallet Event list

These events are fired when some payment is added or deleted from the wallet. There are separate events for credit cards, SEPA and all other uncategorized recurring payment methods. However, for the payment method deletion there is a single event.

* **Credit Card Added**
* **SEPA Payment Method Added**
* **Recurring Payment Method Added**
* **Payment Method Deleted**

## Payment Method Event Payload Structure

| Name | Description | R / O |
| :--- | :---------- | :---: |
| **subject** | `endUser` | R |
| **type** | `deleted` | R |
| **objectId** | EndUser identifier | R |
| **eventDate** | ISO date of the event | R |
| **addedPaymentMethods** `[{object}]` | One element in case of added payment method | O |
| **addedPaymentMethods[].id** | Payment method Id | R |
| **addedPaymentMethods[].status** | Usually `ACTIVATED` | R |
| **addedPaymentMethods[].paymentMethodType** | Broad category type like `CreditCard` etc. | O |
| **addedPaymentMethods[].type** | Specific type `visa`, `mastercard` etc. | R |