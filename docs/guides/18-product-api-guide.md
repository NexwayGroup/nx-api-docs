# API Documentation: Product Management

## Overview

This API allows you to manage your digital products. Products are defined with essential attributes, pricing, and localized content, ensuring they can be displayed properly in a shopping cart. 

While the API includes many attributes, this guide focuses on the minimal setup required to create a product and ensure it appears in the cart UI. Advanced features can be configured later as needed.

---

## Minimal Setup to Create a Product

### Prerequisites
Before creating a product, ensure you have the following identifiers, which are typically provided during your customer account onboarding process:

1. **Customer ID**: The unique identifier of your customer account.
2. **Catalog ID**: The catalog where the product will be listed.
3. **Selling Store ID(s)**: The stores where the product will be sold.

### Steps to Create a Product
1. **Create the product with basic attributes**.
2. When the product is created:
3. **Add a localized description** using the `descriptionId` from the product payload.
4. **Set up a price** using a separate pricing API.
5. **Set up Fulfillment**: to issue activation codes or order confirmation. [Learn more here](15-fulfillment-principles.md).

---

### Minimal Payload for Product Creation

To create a product, use the following endpoint and payload:

**Endpoint**  
`POST /products`

**Payload**
```json
{
  "customerId": "...your-customer-uuid...",
  "catalogId": "...you-catalog-uuid...",
  "sellingStores": ["...your-store-id..."],
  "status": "ENABLED",
  "genericName": "Magic supplement",
  "lifeTime": "PERMANENT",
  "productFamily": "My Game Family",
  "publisherRefId": "ACME-INTERNAL-PID",
  "type": "GAMES",
  "businessSegment": "STRICT_B2C",
  "resources": [
    {
      "url": "https://upload.wikimedia.org/wikipedia/en/8/88/Tobeepornottobeep.jpg",
      "index": 0,
      "label": "product_boxshot"
    }
  ],
  "externalContext": ""
}
```

**Response**
A successful request returns a Location header containing the product ID:
/products/...product_uuid...

### Adding Localized Descriptions

Once the product is created, the response will include a descriptionId. Use this ID to add localized descriptions for the product:

### Payload Example

```json
{
  "id": "b9ae89f8-fada-4097-b8f1-168def77fddc",
  "customerId": "d87b8ec3-867f-4bad-b49c-fedad64afa43",
  "description": "description of product Internal product name",
  "catalogId": "25c4aa71-97fe-40a7-b863-44cd04be0639",
  "fallbackLocale": "en-US",
  "marketingName": "Magic shield",
   "localizedMarketingName": {
      "en-US": "Magic shield"
   },
   "localizedShortDesc": {},
   "localizedLongDesc": {},
   "localizedThankYouDesc": {},
   "localizedPurchaseEmailDesc": {},
   "localizedManualRenewalEmailDesc": {}
}
```

### Setting Up the Price
Prices must be configured separately using the [Pricing API](20-price_api_guide.md). Ensure that a valid price is associated with the product for it to display properly in the cart UI.

### Setting up fulfilment 
Once you setup your fulfilment template (if any, [Learn more here](15-fulfillment-principles.md)). you can send a `PUT` request `/products/{productId}`

```json
{
   "id": "{productId}",
   // ... all other attributes returned by GET /products/{productId} ...
   "fulfillmentTemplate": "{fulfilmentId}"
}
```

## Product attributes 

Below is an in-depth look at the attributes available for product creation.

---

### Core Linking Attributes

#### 1. **Customer ID**
- **Key**: `customerId`
- **Type**: UUID
- **Required**: Yes  
The unique identifier for the customer. This is provided during onboarding and ensures the product is associated with the correct account.

#### 2. **Catalog ID**
- **Key**: `catalogId`
- **Type**: UUID
- **Required**: Yes  
Specifies the catalog in which the product will be listed.

#### 3. **Selling Store IDs**
- **Key**: `sellingStores`
- **Type**: Array of UUIDs
- **Required**: Yes  
A list of store IDs where the product will be available for sale.

---

### Product Lifecycle Attributes

#### 4. **Status**
- **Key**: `status`
- **Type**: Enum
  - `ENABLED`: Active and available for purchase.
  - `DISABLED`: Inactive and hidden from customers.
- **Required**: Yes  

#### 5. **Lifetime**
- **Key**: `lifeTime`
- **Type**: Enum
  - `PERMANENT`: No expiration.
  - `12DAY`: Product license or subscription term is active for 12 days. You can put any number of days like 7 or 15. This is for subscription products.
  - `1MONTH`: Same as days, but for monthly subscriptions. 
  - `1YEAR`: For yearly licenses or subscriptions. 
- **Required**: Yes  

---

### Categorization Attributes

#### 6. **Product Family**
- **Key**: `productFamily`
- **Type**: String
- **Required**: No  
Defines a grouping or category for internal management (e.g., "ACME Product Family").

#### 7. **Type**
- **Key**: `type`
- **Type**: Enum (e.g., `GAMES`, `SOFTWARE`, `CASUAL`, `SERVICE`)
- **Required**: Yes  
Indicates the type of product for compliance and taxation.

#### 8. **Business Segment**
- **Key**: `businessSegment`
- **Type**: Enum
  - `B2C`: Business-to-Consumer.
  - `B2B`: Business-to-Business.
  - `STRICT_B2C`: Will not allow companies to purchase.
  - `STRICT_B2B`: Only companies are allowed to purchase.
- **Required**: No  

---

### Visual Representation and Descriptive Attributes

#### 9. **Generic Name**
- **Key**: `genericName`
- **Type**: String
- **Required**: Yes  
An internal product name for administrative purposes. It is not visible to end users.

#### 10. **Localized Descriptions**
- **Key**: `descriptionId`
- **Type**: UUID  
A unique identifier for managing localized descriptions. Use this to define product names, marketing text, and other content in multiple languages.

#### 11. **Resources**
- **Key**: `resources`
- **Type**: Array of objects
  - `url`: The URL of the resource (e.g., image or video).
  - `index`: Display order.
  - `label`: Descriptive tag for the resource.
- **Required**: No  
Defines assets like box image `product_boxshot` or other images used in the custommized descriptions.

---

### External Attributes

#### 15. **Publisher Reference ID**
- **Key**: `publisherRefId`
- **Type**: String
- **Required**: No  
An optional identifier for linking the product to the customer's systems.

#### 16. **External Context**
- **Key**: `externalContext`
- **Type**: String
- **Required**: No  
A field for additional external metadata, if applicable. Could be used during fulfilment.

---

### Product Localized Description Attributes

#### 1. **Localized Marketing Name**
- **Key**: `localizedMarketingName`
- **Type**: Object
  - Keys: Locale codes (e.g., `en-US`, `fr-FR`).
  - Values: Localized product names.
- **Required**: No  
This field allows customization of product names in different languages.

#### 2. **Localized Short Descriptions**
- **Key**: `localizedShortDesc`
- **Type**: Array of objects (same as above)
- **Required**: No  
Defines text below the marketing name which will be shown in the shopping cart depending on the cart layout.

#### 3. **Localized Long Descriptions**
- **Key**: `localizedLongDesc`
- **Type**: Array of objects (same as above)
- **Required**: No  
Defines long product descriptions which will be used in the shopping cart depending on the cart layout.

#### 4. **Localized Thank You Descriptions**
- **Key**: `localizedThankYouDesc`
- **Type**: Array of objects (same as above)
- **Required**: No  
Customizes text on the `Thank You Page`.

#### 5. **Localized Email Confirmation Descriptions**
- **Key**: `localizedPurchaseEmailDesc`
- **Type**: Array of objects (same as above)
- **Required**: No  
Customizes text in the `Order confirmation email`.
