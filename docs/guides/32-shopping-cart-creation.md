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

| Query Parameter | Description | Required |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **productId** or **product**   | Specifies the product ID from the Nexway catalog. Multiple product IDs can be added  like `checkout/add?productId=e187828b-e7cb-4cdb-bc42-66b51c1fff87&productId=e187828b-e7cb-4cdb-bc42-66b51c1fff88`. | Yes      |
| **layout and theme** | Customizes the checkout layout and design. | No       |
| **locale**       | Defines the checkout language. If not specified, the default store locale applies.                                                     | No       |
| **country**      | Sets the end-user billing address country. Defaults to GeoIP location or the store's default country (depending on your store configuration) if not specified.                                                   | No       |
| **newCart=true**     | Ensures the creation of a new cart instance. If not specified, the system first checks whether a cart already exists based on the given parameters, the end-user's IP, and additional conditions. If a cart is already present, the product from the Buy-Link will be added to it.                                                | No       |
| **scenario**     | Activates a specific checkout flow. Some scenarios may require additional attributes. Scenarios: `acquisition` (default), `retention`, `upgrade`, `subscriptionimport`. | No       |
| **salesFlag**   | Used mostly for tracking purposes. Multiple flags could be provided: `/add?salesFlag=OE,AZ`. The flags will be added to order. | No       |
| **quantity**     | Sets the product quantity. `/add?productId=2f9bb37b-3558-49f0-bea6-69ab834013de&quantity=2`. Will not work if you have multiple products in a buy-link. Use cart api for the case of multiple products customization.     | No       |
| **seats** or **nodes** any price function parameter  | If the product has custom price function parameter (say `seats`), you can specify it like this: `products=a8eee7f3-0e3b-4119-adaf-3151c3f43fc8[seats=50]`.   | No       |
| **lockSelector=true** | Prevents tier or any other product variation parameter (seats, nodes, years, etc.) selection in the UI.            | No       |
| **discount**     | Applies a specified coupon code.  | No       |
| **discountPlan** | Applies a discount plan tag to a subscription.  As a value specify the name of the subscription discount plan tag. `discountStep` specifies the step from which the discount plan should start (by default step count starts at zero). `ignorePurchaseDiscount` (optional) prevents the discount from applying to the initial purchase price, ensuring it only takes effect from the first renewal. By default, the discount applies to the acquisition price as well. Sample: `/add?discountTag=test&discountStep=0&ignorePurchaseDiscount=true`. | No  |
| **isRemotePrice=true** | Fetches a price from an external service. `isRemotePrice=true` activates the functionality `offerId` specifies the identifier in the external service Nexway uses to retrieve the price. If multiple products exist in the Buy-link, the remote price applies only to the first product ID in alphabetical order. Use cart API to fine-tune remote price for each product. Sample: `/add?isRemotePrice=true&offerId=externalOfferId`. | No       |
| **upsell/crosssell=false** | Hides upsell or cross-sell recommendations in the UI. Defaults to true if configured in Nexway.        | No       |
| **optmons=exit** | Optinmonster Pop Up parameter. Allows the presentation of certain marketing ads in the shopping cart. [optmons=exit] leads to popup being raised.                                                                                                                                                                                                                                                                    | No       |                                                                                                                              | **firstName**  and **lastName**       | Pre-fills the buyer's first name and last name fields.  | No       |                                                                                                                                    | No       |
| **email**        | Pre-fills the email field. Use with caution due to personal data exposure. Use cart API to set up obfuscated email address. | No       | 


## Option 2: API Integration 

A shopping cart can also be created via API. The `cartId` is returned in the `Location` header of the API response. To redirect the shopper to checkout, retrieve the cart content using the cartId with the [`GET /carts/{cartId}`](https://apidoc.nexway.store/api/cart#tag/Public/operation/getOne). The response contains `checkoutUrl`, which should be used to redirect the shopper to the checkout page.

### 1. Public Cart Creation API  
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

### 2. Thank you page upgrade offer (cart)  
This endpoint allows upgrading a license immediately after purchase with a more functional product at a higher price. The shopper is charged only the difference between the current and upgraded product prices, without using a prorated formula.
The request should include the ID of the line item from the purchase order.  
[`POST /carts/public/upgrade`](https://apidoc.nexway.store/api/cart#tag/Public/operation/createProductUpgradeCart)  
**API Request Example**
```json
{
    "previousLineItemId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "upgradeProductId": "2f9bb37b-3558-49f0-bea6-69ab834013de"
}  
```


### 3. Cart with Remote Price  
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

### 4. Cart with Discount plan
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
            "tag": "Level1", // The name of the subscription discount plan configured in Nexway
            "discountStep": 0, // Specifies the step (or subscription term) from which the discount should be applied
            "ignorePurchaseDiscount": false //Prevents the discount from applying to the initial purchase price, ensuring it only takes effect from the first renewal
            }
        }
    ]
}   
```
Learn more about subscription plan [here](30-discount-api_guide.md)

### 5. Authenticated Cart
Creates a shopping cart for an authenticated end user with prefilled billing information. The cart is created by a server-to-server API call from the customer’s backend (never from the shopper’s browser) and is intended only for end users who are already authenticated in the customer’s system. Because the end user is known, the checkout can reuse the billing address and payment methods already saved in the end user's wallet, skipping data re-entry.  
[`POST /carts`](https://apidoc.nexway.store/api/cart/#tag/Cart/operation/createItem)

The typical flow is:
1. The authenticated end user clicks a "Buy" button in the customer's application.
2. The customer's server calls [`POST /carts`](https://apidoc.nexway.store/api/cart/#tag/Cart/operation/createItem) using its API key and receives the `cartId` (in the `Location` header) and the `checkoutUrl` (via [`GET /carts/{cartId}`](https://apidoc.nexway.store/api/cart#tag/Public/operation/getOne)).
3. The customer's server generates a Single Sign-On deeplink that points to `checkoutUrl` as the `baseLink`, so the end user lands on the checkout already signed in and with the wallet available.
4. The deeplink is returned to the shopper's browser, which navigates to the pre-authenticated checkout.

See the [Single Sign-On guide](16-single-sign-on.md) for the deeplink generation details.

:::important
`enduserId` **must** be included at the root of the create cart request. It is what binds the cart to the authenticated end user and enables reuse of the saved billing address and wallet payment methods, as well as SSO into the checkout. Without it the cart behaves as an anonymous public cart.
:::

:::note
`country` and `locale` are normally required, but for authenticated carts they are inherited from the end user's profile and can be omitted.
:::

:::note
To let shoppers save replayable payment methods to their wallet, either enable subscriptions on the product or ask your account manager to set `promoteOneClickPayment=true` at the store or customer level. Once enabled, a checkbox appears in the cart allowing the shopper to preserve their payment method for future payments.
:::

**API Request Example**  
```json
--header 'Authorization: Bearer <API_key>'
{
    "enduserId": "{{endUserId}}", // Required — the authenticated end user's Nexway ID
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "wantedProducts": [
        {"id": "2f9bb37b-3558-49f0-bea6-69ab834013de"}
    ]
}   
```
The `Location` header of the response contains the cartId.

**SSO Deeplink Example**  
Once the cart is created, retrieve it via [`GET /carts/{cartId}`](https://apidoc.nexway.store/api/cart#tag/Public/operation/getOne) and take the `checkoutUrl` property from the response — this value must be passed as `baseLink` in the SSO deeplink request so the shopper reaches the checkout already authenticated:
```
POST https://api.nexway.store/iam/deeplinks/
Authorization: Bearer <API_key>
```
```json
{
    "enduserId": "{{endUserId}}",
    "baseLink": "{{cart.checkoutUrl}}", // The checkoutUrl property of the created cart
    "linkDuration": 600,
    "singleUse": true
}
```
The `Location` header of the response contains the deeplink URL to hand back to the shopper's browser.

### 6. Custom Cart
Creates a shopping cart with a custom catalog that is not managed by Nexway. This will create the catalog dynamically in Nexway, and the product IDs will be prefixed with the catalog ID. This method cannot be used to create a cart with subscription products. The method also has other business related limitations. Please discuss usage with your account manager first.  
[`POST /carts/createCheckout`](https://apidoc.nexway.store/api/cart/#tag/Cart/operation/createCheckout)  
**API Request Example**
```json
--header 'Authorization: Bearer <API_key>'
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
            "singleUse": true 
            },
        "products": [
            {
                "id": "product0",
                "name": "Test product",
                "publisherRefId": "123456", // Product reference ID
                "price": {
                    "value": 10.00,
                    "currency": "EUR",
                    "vatIncluded": true 
                    }
            }
        ]
    }
} 
```

### 7. Apply payment method discount
[Here](30-discount-api_guide.md) you can find a description of what a payment method discount is.

In a cart, it is normally applied during `PUT /carts` or `PUT /carts/public`. However, it is also possible to apply it during `POST /carts` or `POST /carts/public`.

The discount is managed via a specific field in the request body: `paymentMethodDiscountId": "{your payment method discount id}"`

There is a validation step that checks the payment method type applied to the cart. Its recurrence must match the one defined in the discount.

For example:
- If a credit card (recurring) is applied to the shopping cart, the payment method discount must have `"recurring": true`.
- If the recurrence does not match, the discount cannot be applied and the backend will return an error.