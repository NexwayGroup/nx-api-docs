
# Order Notifications

## Order statuses

![Order statuses](https://s3storage.nexway.com/iap-staticfiles/2d8a948f601d801a630c9d773f28dba2.png)

## Event list

When the order changes its status an external notification can be sent.
Below is a list of order-related notifications which you can subscribe to.

* Order created
* Order payment failed
* Order completed
* Order completed with error
* Subscription renewal order completed
* Order cancelled

## List of fields

The following attributes will be available in the order related events. If you need to get additional data, which is not available in the event, please refer to the REST API methods to get order or other entities information.

| Name | Description |
| ---- | ----------- |
| id | Order unique identifier |
| status | Order lifecycle status. The status corresponds to the event type. |
| source | Order source: PURCHASE, SUBSCRIPTION, OFFER etc |
| creationDate | Creation date in ISO 8601 format, ex.: 2024-01-01T01:02:03Z |
| currency | Order's Currency, ex.: EUR |
| totalPriceIncVAT | Order total price, including sales tax |
| totalPriceExclVAT | Order total price, excluding sales tax |
| salesFlag | Sales flags is an array of strings provided in the cart. Similar to external context, but unencoded. |
| externalContext | Based64 encoded string of cart parameters |
| decodedExternalContext | Decoded map of cart string parameters if they were provided in the json format |
| payment | Payment object |
| payment.id | Payment id |
| payment.method | Payment method id (visa, mastercard, sepa, visa_electron, visa_inst4, diners, pix, boleto etc.)
| payment.status | Payment status (COMPLETED, FAILED) |
| user | Buyer's details object |
| user.id | Buyer's id |
| user.email | Buyer's email |
| user.firstName | Buyer's first name |
| user.lastName | Buyer's last name |
| user.language | Buyer's language alpha-2 code, ex "pt" |
| user.country | Buyer's country alpha-3 code, ex "BRA" |
| user.street | Buyer's street address |
| user.zipcode | Buyer's postal code |
| user.city | Buyer's city |
| items[] | List of items purchased (products, services, etc.) |
| items[].id | Unique ID for order line item |
| items[].product.name | Product name |
| items[].product.uniqueReference | A unique ID for identifying your product on the Nexway Monetize platform |
| items[].product.publisherReference | A unique ID for identifying your product in your information system, if defined |
| items[].fulfillmentId | Fulfillment process identifier |
| items[].quantity | Product quantity |
| items[].activationCode | Product activation code |
| items[].unitPriceIncVAT | Product unit price, including sales tax |
| items[].unitPriceExclVAT | Product unit price, excluding sales tax |
| items[].VATRate | Sold product applied sales tax rate |
| items[].discountRate | Discount rate applied to product |
| items[].subscription | Some notification definitions like 'Order Completed With Content Including Subscription Data' may also contain information about created subscription. Although, subscription is a separate domain and has it's own [set of events](10-subscriptionNotification.md). |
| items[].subscription.id | SubscriptionId |
| items[].subscription.createDate | Creation date in ISO 8601 format |
| items[].subscription.modelId | Subscription model |
| items[].subscription.storeId | Selling store id |
| items[].subscription.lifecycle | Lifecycle data |
| items[].subscription.lifecycle.id | Lifecycle id string |
| items[].subscription.lifecycle.anniversaryDate | Subscription anniversary (expiration) date in ISO 8601 format |

### Example

```json
{
	"subject": "order",
	"type": "completed",
	"objectId": "3KTEY9K4AAA",
	"eventDate": "2017-08-17T11:25:33.606Z",
	"order": {
		"id": "3KTEY9K4AAA",
		"status": "COMPLETED",
		"creationDate": "2017-08-17T11:25:31Z",
		"payment": {
			"method": "visa",
			"automaticBilling": false
		},
		"totalPriceIncVAT": 357,
		"totalPriceExclVAT": 297.5,
		"externalContext":"eyJzcGFnZSI6==",
		"salesFlag":[
			 "EXTMD_Daily_fr_XXXrenew-30"
		],
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
				"fulfillmentId":"fff994ac-2e29-4dce-a8dd-6c582eee7927",
				"activationCode":"XXXXX-4REZC-CV64B-XXX",
				"quantity": 1,
				"externalContext": "what the customer wants",
				"unitPriceIncVAT": 178.5,
				"unitPriceExclVAT": 148.75,
				"VATRate": 0.2
			}
		]
	}
}
```
