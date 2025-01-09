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
   "localizedManualRenewalEmailDesc": {},
   "variableDescriptions": []
}
```

### Setting Up the Price
Prices must be configured separately using the [Pricing API](20-price_api_guide.md). Ensure that a valid price is associated with the product for it to display properly in the cart UI.

