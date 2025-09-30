# End-user (Buyer) Notifications
These notifications are triggered when the End-user data changes. Mainly payment method or subscription to payment method association.

* **Credit Card Added** - when a credit card is added to the wallet
* **SEPA Added**
* **Payment Method Deteled**
* **EndUser Deleted**

## List of fields

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | end-user unique identifier | R |
| modelId | subscription model id | R |
| name | subscription product name | O |