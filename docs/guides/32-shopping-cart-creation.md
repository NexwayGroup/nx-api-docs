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

## External Context Principles

External context is your own data, carried along with the shopping cart. You set it when the cart is created, Nexway stores it without interpreting it, and you get it back unchanged on the order, the subscription, the notifications and the fulfillment call — typically a session ID, a campaign, a partner reference or a basket ID in your back office.

### 1. Format

The field accepts any string, but the recommended convention is a **Base64-encoded JSON object** — in JavaScript, `JSON.stringify()` followed by `btoa()`:

```js
const externalContext = btoa(JSON.stringify({
    yourContextKey1: "some-value-you-want-to-receive-after-the-purchase"
}));
// "eyJ5b3VyQ29udGV4dEtleTEiOiJzb21lLXZhbHVlLXlvdS13YW50LXRvLXJlY2VpdmUtYWZ0ZXItdGhlLXB1cmNoYXNlIn0="
```

When the value follows this convention, `decodedExternalContext` is exposed alongside `externalContext` in the cart and order notifications and on the subscription object. A value that does not follow it is still transported end to end, but only as the raw `externalContext` string — `decodedExternalContext` stays empty.

### 2. Setting the external context

How the value is produced depends on how the cart is created.

**Cart created via the API.** Send `externalContext` at the root of the request body of [`POST /carts`](https://apidoc.nexway.store/api/cart/#tag/Cart/operation/createItem) or [`POST /carts/public`](https://apidoc.nexway.store/api/cart#tag/Public/operation/publicCreateCart). The value is stored verbatim, so build the Base64 payload yourself.

```json
{
    "country": "FR",
    "locale": "en-US",
    "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
    "externalContext": "eyJ5b3VyQ29udGV4dEtleTEiOiJzb21lLXZhbHVlLXlvdS13YW50LXRvLXJlY2VpdmUtYWZ0ZXItdGhlLXB1cmNoYXNlIn0=",
    "wantedProducts": [
        {"id": "2f9bb37b-3558-49f0-bea6-69ab834013de"}
    ]
}
```

**Cart created from a buy-link.** The cart UI application builds the value from the buy-link URL and passes it to the cart API. It supports two mutually exclusive ways of doing so:

* **Explicit** — the `externalContext` query parameter is passed on the buy-link and is used as is: `/checkout/add?productId={productId}&externalContext={value}`. The value is whatever you put in the URL, not necessarily a Base64-encoded map — it is stored unchanged.
* **Implicit** — the cart UI collects named buy-link query parameters, assembles them into a JSON map and Base64-encodes it. Only named buy-link parameters declared at store level are collected: a parameter whose name is not part of that configuration is ignored, and nothing else (end-user data, cart content, session information) ever enters the map. Ask your account manager to configure `externalContextGenerationParams` at store level with the list of buy-link parameter names you want to be picked up.

Whichever path is used, the cart API treats the value as an atomic string. It never assembles it and never parses it: the only processing it performs is exposing the decoded map as `decodedExternalContext` when the value happens to be a Base64-encoded JSON map.

The value can be overwritten with `PUT /carts` or `PUT /carts/public` as long as the cart is not converted into an order, and the upgrade endpoints accept their own `externalContext` — [`POST /carts/public/upgrade`](https://apidoc.nexway.store/api/cart#tag/Public/operation/createProductUpgradeCart) and the mid-term upgrade request.

### 3. Where you get it back

The external context set on the cart is copied to every object created from it, so you get your own data back without having to keep a mapping table on your side:

| Where you read it back | Field |
|------------------------|-------|
| Cart creation notification | `cart.externalContext` and `cart.decodedExternalContext` |
| Order notification | `order.externalContext` and `order.decodedExternalContext` |
| Order API | `externalContext` on the order and on each line item |
| Subscription | `externalContext` and `decodedExternalContext`, reused for every renewal generated from that subscription |
| Fulfillment | `.Checkout.CartExternalContext` in the [fulfillment template](15-fulfillment-templating.md), so the value can be forwarded to your fulfillment endpoint |

The values carried in the external context are only consumed at the edges of the platform: notifications, fulfillment calls and, in specific configurations, email template customization. They are never read by the platform in between.

### 4. Cart external context vs. product external context

There are two independent external contexts:

* **Cart external context** — set per transaction, as described above. Use it for data that changes from one purchase to another.
* **Product external context** — a static value set on the product itself, at product creation or update only (see [Product API guide](18-product-api-guide.md)). It cannot be set or overridden from the cart. It is carried on the cart line item and on the order line item, and is available in the fulfillment template as `.Product.ExternalContext`. Use it for data that is always the same for a given product, such as a mapping to an SKU or an entitlement code in your system.

Both are transported independently and never overwrite each other.

:::important
The external context is opaque to the platform: it is never used for pricing, tax, fraud or any other business logic, and its content is not validated. Business logic is driven by explicit attributes such as the cart source, the business segment or the scenario — never by the external context.
:::

:::note
Do not put personal data in the external context. On a Buy-Link it is visible in the URL and in browser history, and it is stored and replayed in notifications and fulfillment calls. Keep the payload small — a Buy-Link is subject to the usual URL length limits — and use an opaque identifier that only your system can resolve rather than the data itself.
:::
