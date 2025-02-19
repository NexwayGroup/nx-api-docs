# Building Checkout

Nexway provides multiple integration options for checkout, enabling businesses to seamlessly connect with Nexway’s payment processing, fraud detection, and tax exemption based on their specific needs.
* **Buy-Link Integration**: The quickest and easiest method
* **API Integration**: A more customizable approach for a tailored user experience.

## Option 1: Buy-Link Integration
When shoppers visit your storefront, they can initiate checkout by clicking a UI element containing a Buy-Link in the following format:
```
https://{storeHostName}/checkout/add?products={productId}
```

### Required Parameters
- `storeHostName`: Refers to your brand or website hostname
- `products`: Specifies the product ID from the Nexway catalog.

To add multiple products:
```
https://{storeHostName}/checkout/add?products={productId1}&products={productId2}
```

### Optional Parameters
Optional attributes allow customization of the checkout experience and enable Nexway functionalities. See the Optional Checkout Customization Parameters section for details.


## Option 2: API Integration 

For API-based integration, after a shopper initiates a checkout, you need to:
1. **Create a shopping cart** using the Nexway API
2. **Redirect the shopper** to the checkout URL returned in the API response.


1. **Create a shopping cart** 

There are two available API methods for creating a shopping cart:
- **Standard Cart Creation** (`POST /carts`): Use this if your product catalog is managed within Nexway. This method requires a valid productId from the catalog.
- **Custom Cart Creation** (`POST /carts/createCheckout`): Use this method when selling products that are not in the Nexway catalog. This method creates a new catalog entry and assigns product IDs with the generated catalog as a prefix.

**Standard Cart Creation**

### Required Parameters
- `wantedProducts.id`: An array of product IDs from the Nexway catalog
- Either `storeId` or `storeHostName`: Identifies the store on Nexway side.

### Optional Parameters
See the _Optional Checkout Customization Parameters_ section for details.


**API Request Example**
```json
POST /carts
{
    "storeHostName": "hostname.com",
    "wantedProducts": [
        {
        "id": "2f9bb37b-3558-49f0-bea6-69ab834013de"
        }
    ]
}   
```

**Custom Cart Creation**

### Required Parameters
- `fulCatalog`: Object is used to create a catalog dynamically within Nexway
- `products`: Object is used to define the products to be added to the created catalog.


**API Request Example**
```json
POST /carts/createCheckout
{
  "cart": {
    "fullCatalog": {
        "catalog": {
            "name": "test",
            "status": "ENABLED",
            "type": "INTERNAL",
            "singleUse": true
            },
        "products": [
        {
            "id": "product#1",
            "name": "Test product",
            "publisherRefId": "123456",
            "price": {
                "value": 10.00,
                "currency": "EUR",
                "vatIncluded": true
                }
            }
            ]
        }
    } 
}
```

A successful `201 Created` response includes the `cartId` in the **Location** header.

**API Response Example**
```
Headers
Location: /carts/d513f26a-e36a-4b5d-ab7f-887de69bc21e
```

2. **Redirect the shopper** to the checkout URL returned in the API response.

Retrieve the cart content, using the `cartId` with the following API request:
```
GET /carts/{cartId}
```
The `201 Created` response also contains the `checkoutUrl`, which should be used to redirect the end-user to the checkout page.


## Optional Checkout Customization Parameters
Below is a list of optional parameters for Buy-Link and API that enhance the checkout experience.

### Checkout Properties

**Layout**: Customizes checkout design.

Buy-Link Setup: `/add?layout=aquisition`

API Setup: Add `layout=aquisition` to the checkout URL before redirecting shopper to the checkout page.

**Theme**: Customizes checkout CSS theme.

Buy-Link Setup: `/add?theme=acquisition`

API Setup: Add `theme=acquisition` to the checkout URL before redirecting shopper to the checkout page.

**Locale**: Defines checkout language. If not specified, the default store locale applies.

Buy-Link Setup: `/add?locale=fr-FR`

API Setup:
```json
POST /carts
{
    "locale": "fr-FR",
    ...
}
```

**Country**: Sets checkout country. If not specified, the default store locale applies. If not set, country defaults to the shopper's GeoIP location or default store country.

Buy-Link Setup: `/add?country=FR`

API Setup:
```json
POST /carts
{
    "country": "FR",
    ...
}   
```

**New Cart**: Creates a new cart instance. This option is available only through the Buy-link.

Buy-Link Setup: `/add?newCart=true`

**Scenario**: Activates a specific checkout flow. Depending on the scenario, additional attributes may be required. For example, in retention scenario, a reference to the previous licenseId, order, or user account is typically needed.

Buy-Link Setup: `/add?scenario=retention&licenseId=123456789`

API Setup:
```json
POST /carts
{
    "scenario": "retention",
    "wantedProducts": [
		{
			"licenseId": "123456789",
            ...
            }
	]
}   
```

**Sales flag**: Activates sales performance tracking.

Buy-Link Setup: `/add?salesFlag=OE,AZ`

API Setup:
```json
POST /carts
{
   "salesFlags": [
		"OE",
		"AZ"
	],
    ...
}   
```

**External Context**: Enables customers to pass information to the cart and subsequently to the order. This information can then be forwarded to fulfillment, allowing your service to track the order or include any specific details with it. 
This option is available only through the API.

API Setup:
```json
POST /carts
{
    "externalContext": "base64(\"channel\": \"online\", \"licensenumber\": \"123456789\")",
    ...
}   
```

### Product properties

**Quantity**: Sets product quantity.

Buy-Link Setup: `/add?product=2f9bb37b-3558-49f0-bea6-69ab834013de[quantity=2]`

API Setup:
```json
POST /carts
{
    "wantedProducts": [
		{
			"quantity": 2,
            ...
			}
    ]
}   
```

**Tiers**: Specifies product tiers.

Buy-Link Setup: `/add?product=2f9bb37b-3558-49f0-bea6-69ab834013de[tier=20]`

API Setup:
```json
POST /carts
{
    "wantedProducts": [
        {
            "priceFunctionParameters": {
                "tier": "20"
            },
            ...
        }
    ]
}   
```

**Lock Selector**: 	Prevents tier selection on UI.

Buy-Link Setup: `/add?&lockSelector=true`

API Setup: Add `lockSelector=true` to the checkout URL before redirecting shopper to the checkout page.

**license id**: Assigns external license ID.

Buy-Link Setup: `/add?licenseId=123456789`

API Setup:
```json
POST /carts
{
    "wantedProducts": [
		{
			"licenseId": "123456789",
            ...
            }
	]
}   
```

### Price management

**Discount**: Applies specified discount.

Buy-Link Setup: `/add?discounts=sale`

API Setup:
```json
POST /carts
{
    "discounts": [ "sale" ],
    ...
}   
```

**Discount Plan**: Applies discount plan to subscription.
This setup requires the following parameters:
- `discountTag` (required): The name of the subscription discount plan 
- `discountStep` (required): Specifies the step (or subscription term) from which the discount should be applied. The step count starts at zero
- `ignorePurchaseDiscount` (optional): Prevents the discount from applying to the initial purchase price, ensuring it only takes effect from the first renewal. By default, the discount applies to the acquisition price as well.

Buy-Link Setup: `/add?discountTag=test&discountStep=0&ignorePurchaseDiscount=true`

API Setup:
```json
POST /carts
{
    "wantedProducts": [
            {
                "discountPlan": {
                "tag": "DemandeLevel1",
                "discountStep": 0,
                "ignorePurchaseDiscount": false
                },
                ...
        ]
}   
```

**Remote price**: Applies the price from an external service.
To enable this setup, the following parameters must be provided:
-`isRemotePrice` (required): Activates the functionality to fetch the price from the external service
- `offer-id` (required): Specifies the identifier in the external service that Nexway uses to retrieve the price
Additional parameters may be required based on your service's integration details.

Buy-Link Setup: `/add?isRemotePrice=true&offer-id=externalOfferId`

API Setup:
```json
POST /carts
{
    "wantedProducts": [
            {
                "isRemotePrice": true,
                "offerId": "externalOfferId",
                ...
                }
        ]
}   
```

### Marketing management

**Upsell/Cross-sell**: Hides upsell or cross-sell recommendations on UI. By default, they will be shown if they are configured in Nexway.

Buy-Link Setup: `/add?upsell=false&crossell=false`

API Setup:
```json
POST /carts
{
    "hideUpSell": true,
    "hideCrossSell": true,
    ...
}   
```

**Marketing ads**: Displays pre-configured marketing ads.

Buy-Link Setup: `/add?optmons=exit`

API Setup: Add `optmons=exit` to the checkout URL before redirecting shopper to the checkout page.

### Shopper Information Pre-Fill

**Name**: Pre-fills first name, last name.

Buy-Link Setup: `/add?firstName=John&lastName=Smith`

API Setup:
```json
POST /carts
{
    "endUser": { 
        "firstName": "John",
	    "lastName": "Smith",
        ...
	}
}   
```

**Email**: Pre-fills email. It can be obfuscated or set to read-only, with the read-only option available only through a buy-link.

Buy-Link Setup: 
```
/add?firstName=John&lastName=Smith
/add?&email=John.Smith@domain.com[obfuscated=true]
/add?email=John.Smith@domain.com[readonly=true]
```

API Setup:
```json
POST /carts
{
    "endUser": { 
		"email": "John.Smith@domain.com",
		"maskedEmail": true,
        ...
	}
}   
```

**Billing address**: Pre-fills billing address. This option is available only through the API.

API Setup:
```json
POST /carts
{
    "endUser": {
        "city": "Canberra",
        "country": "AU",
        "streetAddress": "Queen Street, 12",
        "zipCode": "2601",
        ...
    }
}   
```

**Company info**: Pre-fills company info. This option is available only through the API.

API Setup:
```json
POST /carts
{
    "endUser": {
        "company": {
            "companyName": "ABC",
            "vatNumber": "12 345 678 912"
        },
        ...
    },
}   
```