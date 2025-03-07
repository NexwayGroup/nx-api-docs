# Shopping Cart Creation Guide

Nexway provides a few integration options for creating a shopping cart, allowing businesses to seamlessly connect with Nexway’s payment processing, fraud detection, and tax management solutions based on their specific requirements.

## Integration Options
* **Buy-Link Integration**: The quickest and easiest method
* **API Integration**: A more customizable approach for a tailored user experience.

## Option 1: Buy-Link Integration
A shopping cart is created when the shopper clicks on the buy-link, automatically redirecting them to the checkout page.
To generate a shopping cart using a Buy-Link, use the following format:
```
https://{storeHostName}/checkout/add?productId={productId}
```
### Buy-link Parameters

| Property         | Description                                                                                                                                                                                                                                                                                                       | Required | Buy-Link Setup |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------|
| **storeHostName** | Refers to your brand or website hostname                                                                                                                                                                                                                                                                       | Yes      | `https://www.yourBrandName.store/checkout/` |
| **productId**    | Specifies the product ID from the Nexway catalog. Multiple product IDs can be added                                                                                                                                                                                                                             | Yes      | `checkout/add?productId=e187828b-e7cb-4cdb-bc42-66b51c1fff87&productId=e187828b-e7cb-4cdb-bc42-66b51c1fff88` |
| **Layout and Theme** | Customizes the checkout layout and design                                                                                                                                                                                                                                                         | Optional | `/add?layout=acquisition&theme=acquisition` |
| **Locale**       | Defines the checkout language. If not specified, the default store locale applies                                                                                                                                                                                                                              | No       | `/add?locale=fr-FR` |
| **Country**      | Sets the checkout country. Defaults to GeoIP location or the store's default country if not specified                                                                                                                                                                                                        | No       | `/add?country=FR` |
| **New Cart**     | Creates a new cart instance. Available only through the Buy-link                                                                                                                                                                                                                                               | No       | `/add?newCart=true` |
| **Scenario**     | Activates a specific checkout flow. Some scenarios may require additional attributes                                                                                                                                                                                                                          | No       | `/add?scenario=retention&licenseId=123456789` |
| **Sales Flag**   | Activates sales performance tracking                                                                                                                                                                                                                                                                           | No       | `/add?salesFlag=OE,AZ` |
| **Quantity**     | Sets the product quantity                                                                                                                                                                                                                                                                                      | No       | `/add?productId=2f9bb37b-3558-49f0-bea6-69ab834013de&quantity=2` |
| **Tiers**        | Specifies product tiers                                                                                                                                                                                                                                                                                        | No       | `/add?productId=2f9bb37b-3558-49f0-bea6-69ab834013de&tier=20` |
| **Lock Selector** | Prevents tier selection in the UI                                                                                                                                                                                                                                                                            | No       | `/add?lockSelector=true` |
| **License ID**   | Assigns an external license ID                                                                                                                                                                                                                                                                                 | No       | `/add?licenseId=123456789` |
| **Discount**     | Applies a specified discount (used only for coupons)  | No       | `/add?discounts=sale` |
| **Discount Plan** | Applies a discount plan to a subscription.  `discountTag` specifies the name of the subscription discount plan. `discountStep`specifies the step (or subscription term) from which the discount should apply (step count starts at zero). `ignorePurchaseDiscount` (optional) prevents the discount from applying to the initial purchase price, ensuring it only takes effect from the first renewal. By default, the discount applies to the acquisition price as well | No | `/add?discountTag=test&discountStep=0&ignorePurchaseDiscount=true` |
| **Remote Price** | Fetches a price from an external service. `isRemotePrice=true` activates the functionality `offer-id` specifies the identifier in the external service Nexway uses to retrieve the price. If multiple products exist in the Buy-link, the remote price applies only to the first product ID in alphabetical order | No       | `/add?isRemotePrice=true&offer-id=externalOfferId` |
| **Upsell/Cross-sell** | Hides upsell or cross-sell recommendations in the UI. Defaults to showing if configured in Nexway                                                                                                                                                                              | No       | `/add?upsell=false&crosssell=false` |
| **Marketing Ads** | Displays pre-configured marketing ads                                                                                                                                                                                                                                                                        | No       | `/add?optmons=exit` |
| **Name**         | Pre-fills the first name and last name fields                                                                                                                                                                                                                                                                | No       | `/add?firstName=John&lastName=Smith` |
| **Email**        | Pre-fills the email field `obfuscated=true` obfuscates the email. `readonly=true` sets the email to read-only (only available through a Buy-link) | No       | `/add?email=John.Smith@domain.com&obfuscated=true`<br>`/add?email=John.Smith@domain.com&readonly=true` |


## Option 2: API Integration 

A shopping cart can also be created via API. The `cartId` is returned in the `Location` header of the API response. To redirect the shopper to checkout, retrieve the cart content using the cartId with the [`GET /carts/{cartId}`](https://apidoc.nexway.store/api/cart#tag/Public/operation/getOne). The response contains checkoutUrl, which should be used to redirect the end-user to the checkout page.

### API Use Cases 

**1. Standard Cart** 
Creates a cart using products from the Nexway catalog.  
[`POST /carts/public`](https://apidoc.nexway.store/api/cart#tag/Public/operation/publicCreateCart)  
**API Request Example**
```json
{
    "country": "FR",
    "locale": "en-US",
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "wantedProducts": [
        {"id": "2f9bb37b-3558-49f0-bea6-69ab834013de"}
    ]
}   
```

**2. Product Upgrade Cart** 
This endpoint allows upgrading a license immediately after purchase with a more functional product at a higher price. The shopper is charged only the difference between the current and upgraded product prices, without using a prorated formula.
The request should include the ID o the line item from the purchase order.  
[`POST /carts/public/upgrade`](https://apidoc.nexway.store/api/cart#tag/Public/operation/createProductUpgradeCart)  
```json
{
    "previousLineItemId": "36f48867-d6ca-42d3-bf55-5f54a6740803", // id of the lineItem from the purchase order 
    "upgradeProductId": "2f9bb37b-3558-49f0-bea6-69ab834013de"
}  
```


**3. Cart with Remote Price** 
Creates a shopping cart with a product price retrieved from an external service.  
[`POST /carts/public`](https://apidoc.nexway.store/api/cart#tag/Public/operation/publicCreateCart)  
**API Request Example**
```json
{
    "country": "FR",
    "locale": "en-US",
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "wantedProducts": [
        {
        "id": "2f9bb37b-3558-49f0-bea6-69ab834013de",
        "isRemotePrice": true,
	    "offerId": "1234567890" // Identifies the external price source
        }
    ]
}   
```

**4. Cart with Discount plan** 
Creates a shopping cart with a discount plan applied to the subscription. The `tag` and `discountStep` attributes are required.  
[`POST /carts/public`](https://apidoc.nexway.store/api/cart#tag/Public/operation/publicCreateCart)  
**API Request Example**
```json
{
    "country": "FR",
    "locale": "en-US",
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "wantedProducts": [
        {
        "id": "2f9bb37b-3558-49f0-bea6-69ab834013de",
		"discountPlan": {
            "tag": "SaleLevel1", // The name of the subscription discount plan configured in Nexway
            "discountStep": 0, // Specifies the step (or subscription term) from which the discount should be applied
            "ignorePurchaseDiscount": false //Prevents the discount from applying to the initial purchase price, ensuring it only takes effect from the first renewal
            }
        }
    ]
}   
```

**5. Authorized Cart**  
Creates a shopping cart with prefilled shopper's billing information.  
[`POST /carts/public`](https://apidoc.nexway.store/api/cart#tag/Cart/operation/createItem)  
**API Request Example**
--header 'Authorization: Bearer <API_key>'
```json
{
    "country": "FR",
    "locale": "fr-FR",
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "wantedProducts": [
        {
        "id": "2f9bb37b-3558-49f0-bea6-69ab834013de"
        }
    ],
    "endUser": { 
        "firstName": "John",
        "lastName": "Smith",
        "email": "John.Smith@domain.com",
        "maskedEmail": true,
        "city": "Marseille",
        "country": "FR",
        "streetAddress": "Queen Street, 12",
        "zipCode": "2601"
        // Add more billing information if necessary.
    }
}   
```

**6. Custom Cart** 
Creates a shopping cart with a custom catalog that is not managed by Nexway. This will create the catalog dynamically in Nexway, and the product IDs will be prefixed with the catalog ID. This method cannot be used to create a cart with subscription products.  
[`POST /carts/createCheckout`](https://apidoc.nexway.store/api/cart#tag/Cart/operation/createItem)  
**API Request Example**
--header 'Authorization: Bearer <API_key>'
```json
{
    "cart": {
        "locale": "fr-FR",
        "country": "FR",
        "storeId": "6fdd8d20-b31b-4e24-83c1-30681f112b5c",
        "products": {
            "product0": {
                 "quantity": 1
            }
        }
    },
    "fullCatalog": {
        "catalog": {
            "name": "Catalog#1", 
            "status": "ENABLED", 
            "type": "INTERNAL", 
            "singleUse": true // Whether the catalog is single-use
            },
        "products": [
            {
                "id": "product0",
                "name": "Test product",
                "publisherRefId": "123456", // Product reference ID
                "price": {
                    "value": 10.00,
                    "currency": "EUR",
                    "vatIncluded": true // Indicates whether VAT is included in the price.
                    }
            }
        ]
    }
} 
```
