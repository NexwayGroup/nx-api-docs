# End-user (Buyer) Notifications

These notifications are triggered when the End-user data changes. Mainly when payment method is added or deleted or subscription to payment method association.

## Notification Events

* **EndUser Deleted** - when an end-user account is deleted

## EndUser Deleted Payload Structure

| Name | Description | R / O |
| :--- | :---------- | :---: |
| **subject** | For order notifications the subject is `endUser` | R |
| **type** | `deleted` | R |
| **objectId** | EndUser identifier | R |
| **eventDate** | ISO date of the event | R |



The following notifications are WIP:
* **Credit Card Added** - when a credit card is added to the wallet
* **SEPA Added** - when a SEPA payment method is added
* **Payment Method Deleted** - when a payment method is removed from the wallet


