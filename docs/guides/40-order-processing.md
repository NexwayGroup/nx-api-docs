# Order processing

An **order** contains  a list of purchased products, pricing, discounts and the  address of the shopper. **Nexway Monetize** automatically generates an order when a shopper checks out the cart and starts the order processing workflow. The order then progresses through a specific workflow until its eventual completion.

## Order workflow
The diagram below shows the main statuses of the order workflow.

![Order statuses](https://s3storage.nexway.com/iap-staticfiles/2d8a948f601d801a630c9d773f28dba2.png)


## Order Statuses

Each order has a **status**, updated in real-time.

Order statuses fall into **temporary** or **permanent** categories:

|  Status  |  Description  |
|  ------  |  -----------  |
| **CREATED** *(Temporary)* | The system has just created an order from a cart. This status appears immediately after the shopper validates their purchase. Subscription prebilling order also rests in the **CREATED** status until the first charge attempt. |
| **PAYMENT_PENDING** *(Temporary)*   | The order is awaiting payment. The payment process has been initiated. This status often applies to asynchronous payment methods (PayPal, bank transfers, cheques, SEPA, etc.). It updates when Nexway receives payment confirmation.   |
| **PAYMENT_REFUSED** *(Permanent)*  | The payment for the order was not approved due to reasons such as insufficient funds, blocked online transactions, incorrect card details, or an expired card. Another payment attempt can be initiated and order could be completed.   |
| **FRAUD_PENDING** *(Temporary)*   | The order has been flagged as possibly fraudulent after Nexway's automated fraud screening. It is manually reviewed during office hours. After review, it is either: (1) accepted and fulfilled, (2) rejected and cancelled, or (3) remains pending while awaiting shopper authentication (up to 6 days before automatic cancellation). |
| **PENDING** *(Temporary)*   | The order is being processed and various steps required for fulfillment are underway.   |
| **PENDING_WITH_ERROR** *(Temporary)*   | Processing errors prevent full completion, such as invoice generation failures or issues sending confirmation emails. The process will try to repair automatically.   |
| **PARTIAL_COMPLETED** *(Temporary)*   | The order can only be partially fulfilled due to errors (similar to "Pending with errors"). The process can be repaired. Fulfilment errors usually lead to partially completed orders.    |
| **COMPLETED** *(Permanent)*   | The order has been fully processed, payment received, and all completion criteria met (e.g., subscription activation, confirmation email sent with order details, access credentials, and invoice).  |
| **CANCEL_PENDING** *(Temporary)* | A cancellation request has been entered, and the order cancellation is being processed.    |
| **ABORTED** *(Permanent)*   | Applies to orders with asynchronous payment methods. The order is stopped because payment was not received within the allotted time. Aborted orders are not fulfilled.  |
| **CANCELED** *(Permanent)*   | The order has been cancelled with some specific cancellation reason. Cancelled orders are refunded.   |
| **CANCELED_WITH_ERROR** *(Permanent)*   | The order was cancelled, but errors occurred during the cancellation process, such as failure to send a confirmation email. This process can be repaired.  |
| **PARTIAL_CANCELED** *(Temporary)*  | Part of the order has been cancelled and refunded (e.g., an order with 3 products where 1 was cancelled and refunded).  |

> **Note:** A "Completed" order can still be "Cancelled," for example, if the shopper exercises their right of withdrawal within the allowed time frame.

