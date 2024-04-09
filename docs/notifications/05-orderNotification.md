
# Order Notifications

You can receive notifications whenever your order status changes.

## Order statuses
The diagram below shows the main statuses of the order workflow.

![Order statuses](https://s3storage.nexway.com/iap-staticfiles/2d8a948f601d801a630c9d773f28dba2.png)

## Event list

Below is a list of order-related notifications which you can subscribe to:

* Order created
* Order payment failed
* Order payment refused
* Order completed
* Order completed with error
* Order fulfillment failed
* Subscription renewal order completed
* Order cancelled

## List of fields

The notification payload will include an 'order' object containing the following details:

| Name | Description | R/O |
| ---- | ----------- | --- |
| id | Order unique identifier | R |
| status | Order status corresponds to the event type. | R |
| source | Order source: PURCHASE, SUBSCRIPTION, OFFER, MANUAL_RENEWAL, etc | R |
| creationDate | Creation date in ISO 8601 format, ex.: 2024-01-01T01:02:03Z | R |
| currency | Order's Currency, ex.: EUR | R |
| totalPriceIncVAT | Order total price, including sales tax | R |
| totalPriceExclVAT | Order total price, excluding sales tax | R |
| salesFlag | Sales flags is an array of strings provided in the cart. Similar to external context, but unencoded. | O |
| externalContext | Based64 encoded string of cart parameters | O |
| decodedExternalContext | Decoded map of cart string parameters if they were provided in the json format | O |
| payment | Payment object | O |
| payment.id | Payment id | R |
| payment.method | Payment method id (visa, mastercard, sepa, visa_electron, visa_inst4, diners, pix, boleto etc.) | O |
| payment.status | Payment status (COMPLETED, FAILED) | R |
| user | Buyer's details object | O |
| user.id | Buyer's id | O |
| user.email | Buyer's email | R |
| user.firstName | Buyer's first name | O |
| user.lastName | Buyer's last name | O |
| user.language | Buyer's language alpha-2 code, ex "pt" | O |
| user.country | Buyer's country alpha-3 code, ex "BRA" | O |
| user.street | Buyer's street address | O |
| user.zipcode | Buyer's postal code | O |
| user.city | Buyer's city | O |
| items[] | List of items purchased (products, services, etc.) | R |
| items[].id | Unique ID for order line item | R |
| items[].product.name | Product name | R |
| items[].product.uniqueReference | A unique ID for identifying your product on the Nexway Monetize platform | R |
| items[].product.publisherReference | A unique ID for identifying your product in your information system, if defined | O |
| items[].fulfillmentId | Fulfillment process identifier | O |
| items[].quantity | Product quantity | R |
| items[].activationCode | Product activation code | O |
| items[].unitPriceIncVAT | Product unit price, including sales tax | R |
| items[].unitPriceExclVAT | Product unit price, excluding sales tax | R |
| items[].VATRate | Sold product applied sales tax rate | R |
| items[].discountRate | Discount rate applied to product | O |
| items[].subscriptionId | SubscriptionId if the line item has one | O |

If you need to get additional data, which is not available in the event, please refer to the REST API methods to get order or other entities.

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
			"id": "037dbcbb-c5bd-4a81-8fd7-3420b440fdf3",
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
				"subscriptionId":"fffdddac-2e29-4dce-a8dd-052e25968b8b",
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

### Additional fields for 'Order Completed With Content Including Subscription Data' notification

Certain notification types, like 'Order Completed with Subscription Data' and 'Order Cancelled with Subscription Data' include details about the subscription. Although, subscriptions are a separate domain with thir own [set of events](10-subscriptionNotification.md).

| Name | Description | R/O |
| ---- | ----------- | --- |
| items[].subscription | Subscription object | O |
| items[].subscription.id | SubscriptionId | R |
| items[].subscription.createDate | Creation date in ISO 8601 format | R |
| items[].subscription.modelId | Subscription model | R |
| items[].subscription.storeId | Selling store id | R |
| items[].subscription.lifecycle | Lifecycle data | R |
| items[].subscription.lifecycle.id | Lifecycle id string | O |
| items[].subscription.lifecycle.anniversaryDate | Subscription anniversary (expiration) date in ISO 8601 format | R |
