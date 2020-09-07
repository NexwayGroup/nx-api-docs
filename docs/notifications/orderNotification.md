Navigation: Notifications/Order Notifications
SortOrder: 220

# Order Notifications

## Lifecycle

![Order Lifecycle](/nexway-monetize/static/orderLifecyle.png)

## Event list

The following is a list of all Order-related notifications that you can choose to receive during a purchase workflow.

* Order creation
* Transaction call
* Fraud call
* Order creation
* Subscription call
* Subscription
* Fulfillment call
* Fulfillment
* Activation code
* Update subscription with fulfillment
* Update end-user with subscription
* Order confirmation
* Invoice creation
* End-user creation
* End-user retrieved by email
* Informative event
* Order cancellation
* Cancellation
* Partial cancellation
* Fulfillment cancellation
* Subscription cancellation
* Credit note creation

## List of fields

This is how both email and webhook notifications are structured:

| Name | Description |
| ---- | ----------- |
| id | Order unique identifier |
| status | Functional status |
| creationDate | Creation date |
| currency | Order's Currency |
| totalPriceIncVAT | Order total price, including sales tax |
| totalPriceExclVAT | Order total price, excluding sales tax |
| payment.method | Payment method (Visa, Mastercard, PayPal, SEPA, etc.)
| payment.automaticBilling | Payment can be renewed (true/false) |
| user.email | Buyer's email |
| user.firstName | Buyer's first name |
| user.lastName | Buyer's last name |
| user.language | Buyer's language |
| user.country | Buyer's country |
| user.street | Buyer's street address |
| user.zipcode | Buyer's zipcode |
| user.city | Buyer's city |
| items[] | List of items purchased (products, services, etc.) |
| items[].id | Unique ID for order line item|
| items[].product.name | Product name |
| items[].product.uniqueReference | A unique ID for identifying your product on the Nexway Monetize platform |
| items[].product.publisherReference | A unique ID for identifying your product in your information system |
| items[].quantity | Product quantity |
| items[].unitPriceIncVAT | Product unit price, including sales tax |
| items[].unitPriceExclVAT | Product unit price, excluding sales tax |
| items[].externalContext  | Product external context |
| items[].VATRate | Sold product sales tax amount |
| items[].discountRate | Discount rate applied to product |

### Example

```json
{
	"subject": "order",
	"type": "orderConfirmation",
	"objectId": "17101797d2bb",
	"eventDate": "2017-08-17T11:25:33.606Z",
	"order": {
		"id": "17101797d2bb",
		"status": "COMPLETED",
		"creationDate": "2017-08-17T11:25:31Z",
		"payment": {
			"method": "visa",
			"automaticBilling": false
		},
		"totalPriceIncVAT": 357,
		"totalPriceExclVAT": 297.5,
		"currency": "USD",
		"user": {
			"email": "jdoe@com2us.com",
			"firstName": "John",
			"lastName": "Doe",
			"language": "en",
			"country": "USA",
			"street": "587 Main Street",
			"zipcode": "20005",
			"city": "Washington"
		},
		"items": [
			{
				"id": "fd58a2e5-548c-44a7-b78d-5b246c1a25cd",
				"product": {
					"name": "My product name",
					"uniqueReference": "82165493-486f-54fa-a454-65458da64c53",
					"publisherReference": "SKU-0001",
				},
				"quantity": 1,
				"externalContext": "what the customer wants"
				"unitPriceIncVAT": 178.5,
				"unitPriceExclVAT": 148.75,
				"VATRate": 0.2
			}
		]
	}
}
```
