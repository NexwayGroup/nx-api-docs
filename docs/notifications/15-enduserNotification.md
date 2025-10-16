# End-user (Buyer) Notifications

These notifications are triggered when the End-user data changes. 

## Event list

* **EndUser Deleted**  
when an end-user account is deleted

## EndUser Deleted Payload Structure

| Name | Description | R / O |
| :--- | :---------- | :---: |
| **subject** | `endUser` | R |
| **type** | `deleted` | R |
| **objectId** | EndUser identifier | R |
| **eventDate** | ISO date of the event | R |


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