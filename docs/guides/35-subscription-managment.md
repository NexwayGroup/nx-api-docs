# Subscription management


## Subscription Statuses

Every subscription always has a specific status. Below is a table describing each status and its lifecycle.


| Status | Description |
|--------|-------------|
| **Active** | An active, paid subscription. |
| **Trial** | A free trial period. |
| **Dunning** | The period of payment attempts. This may include a grace period. It precedes the `Expired` status if payment ultimately fails. |
| **Renewing** | The standard renewal process for an `Active` subscription. |
| **TrialConversion** | The renewal process (conversion to paid) for a `Trial` subscription. Similar to `Renewing`. |
| **Suspended** | A "soft" cancellation. The subscription can be reactivated before its expiration date. |
| **Canceled** | **Terminal status.** The subscription was canceled by the user and will not be renewed. |
| **Expired** | **Terminal status.** The subscription expires after the grace period ends and all payment attempts during the `Dunning` phase have failed. Subscription `Expiration date` is the date when grace period starts so the status of the subscription doesn't change to `Expired` at the `Expiration date`. |

## Subscription Lifecycle

Subscription statuses can change according to the following main scenarios:

1. New trial subscription:
   `Trial` -> `TrialConversion` -> `Active`

2. Active subscription renewal:
   `Active` -> `Dunning` -> `Renewing` -> `Active`

3. Failed renewal:
   `Dunning` -> `Expired`

4. Suspending and reactivating:
   `Active` -> `Suspended` -> `Active` or
   `Suspended` -> `Expired`

**Special Transitions:**
* **Cancellation and Suspension:** A subscription can be moved to `Canceled` from any status. `Suspended` is a soft-cancellation status meaning that subscription will not be renewed automatically. A `Suspended` subscription is considered active and paid until the `ExpirationDate`.
* **Terminal Statuses:** `Canceled` and `Expired` are final. Once a subscription enters these statuses, it cannot be reactivated.

**Note on Payment Methods:**
* Some subscriptions may be linked to non-recurring payment methods. Such subscriptions will not be able to auto-renew, although the system will attempt to process the renewal until it fails and moves to the `Expired` status.


# Subscription Retention & Flexibility Features

The subscription scenarios outlined below are designed to enhance the user experience, boost shopper retention, and provide greater flexibility in managing subscription options.

## Stay Subscribed Offer
The Stay Subscribed Offer flow enables you to offer a discount on the upcoming subscription renewal to shoppers who wish to cancel their subscription's auto-renewal. This offer cannot be applied to subscriptions already in the renewal billing period or those in the trial period. When shopper accepts the offer, the discount is applied to their subscription renewal price, which will be charged during the auto-renewal process. Shopper do not need to pay immediately when they accept the offer.

To use this feature, a [discount](30-discount-api_guide.md) with `source = OFFER` and `offerSubSource = SUSPEND` must first be configured in Nexway. Please note that the discount for Stay Subscribed flow can only be defined as a percentage value, not as an absolute amount.

**Integration options**

Nexway provides two integration options to suit your business needs:

### Option 1: Using the Nexway-hosted End-User portal


This is the simplest integration method. Nexway's portal presents the Stay Subscribed offer directly to shoppers. When shopper logs into their account on the Nexway End-User portal and attempts to cancel auto-renewal, they will see the offer. If they accept it, the discount will be applied to their upcoming subscription renewal, and a prebilling order will be created.


### Option 2: Integrating Nexway API

This option allows full control through your platform's user interface. 

**Part 1. Create a Stay Subscribed Offer**

When a shopper selects the option to cancel auto-renewal, your platform sends a request to the Nexway API to create the Stay Subscribed offer. This request includes the Nexway subscription identifier and, optionally, the [discount](30-discount-api_guide.md) if you wish to explicitly specify the discount. 

**API Request Example**
```json
POST /carts/subscription-offers
{
    "subscriptionId": "18fc54b1-d07e-4d65-9f1f-d1ae6621a813",
    "discountCode": "StaySubscribeddiscount" //optional
}
```
The API searches for the Stay Subscribed discount and applies it to the subscription renewal price. A `201 Created` response returns a cart object with `source = OFFER` and `offerSubSource = SUSPEND`. Response contains the offer details, including the discounted price, which you may use to display the offer on your user interface.

**API Response Example**
```json
{
    "cartId": "86695c69-4d49-4208-8444-1e482a4d0150",
    "subscriptionId": "18fc54b1-d07e-4d65-9f1f-d1ae6621a813",
    "customerId": "27213244-fa24-4728-ae03-a42376aadd42",
    "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
    "source": "OFFER",
    "offerSubSource": "SUSPEND",
    "totalAmount": 180.0,
    "product": {
        "id": "e1978002-ffa4-4c50-9635-e20239b3a27d",
        "name": "Test Product",
        "publisherRefId": "111047E9753",
        "lifeTime": "1MONTH",
        "price": {
            "currency": "AUD",
            "netPrice": 181.82,
            "grossPrice": 200.0,
            "vatIncluded": true,
            "vatRate": 0.1,
            "vatAmount": 18.18,
            "discountedPrice": {
                "discountId": "0605770c-538a-49bf-997a-b08d4ed88a23",
                "discountedNetPrice": 163.64,
                "discountedGrossPrice": 180.0,
                "netPriceDiscountAmount": 18.18,
                "grossPriceDiscountAmount": 20.0,
                "vatDiscountAmount": 16.36,
                "discountRate": 0.1,
                "allDiscountsApplied": [
                    {
                        "grossPrice": 200.0,
                        "netPrice": 181.82,
                        "discountedGrossPrice": 180.0,
                        "discountedNetPrice": 163.64,
                        "discountRate": 0.1,
                        "vatDiscountAmount": 16.36,
                        "netPriceDiscountAmount": 18.18,
                        "grossPriceDiscountAmount": 20.0,
                        "discountId": "0605770c-538a-49bf-997a-b08d4ed88a23",
                        "model": "COUPON",
                        "discountCode": "StaySubscribeddiscount"
                    }
                ]
            }
        }
    },
    "warnings": []
}
```
**Part 2. Create an Order**
When the shopper accepts the Stay Subscribed offer, your platform must create an order by making the following request providing Nexway subscription identifier and cart identifier from the response of the previous call. 

**API Request Example**
```json
POST /purchases/prepareNextOrderFromOffer
{
  "cartId": "86695c69-4d49-4208-8444-1e482a4d0150",
  "subscriptionId": "18fc54b1-d07e-4d65-9f1f-d1ae6621a813"
}
```
The API converts the cart to order which will be used during auto-renewal. A `201 Created` response confirms the order with `source = OFFER` and `offerSubSource = SUSPEND` is created. You can display a confirmation page to the shopper after receiving this response.

**API Response Example**
```json
{
    "id": "40M63XUA7GU",
    "createDate": 1733917173779,
    "lineItems": [
        {
            "id": "8e497d23-fe7e-4816-a3a5-9fa24d4d640b",
            "productId": "e1978002-ffa4-4c50-9635-e20239b3a27d",
            "lifeTime": "1MONTH",
            "licenseNextExpirationDate": 1768123641000,
            "trial": false,
            "trialDuration": 0,
            "name": "Test Product",
            "subscriptionTemplate": "NEXWAY_1M",
            "publisherRefId": "111047E9753",
            "productType": "SOFTWARE",
            "expirationDate": 1768123641000,
            "amount": 180.0,
            "currency": "AUD",
            "vatRate": 0.1,
            "netAmount": 163.64,
            "vatAmount": 16.36,
            "discountRate": 0.1,
            "initialAmount": 200.0,
            "initialNetAmount": 181.82,
            "initialVatAmount": 18.18,
            "price": {
                "vatAmount": 18.18,
                "netPrice": 181.82,
                "grossPrice": 200.0,
                "vatRate": 0.1,
                "currency": "AUD",
                "discountedPrice": {
                    "discountedNetPrice": 163.64,
                    "discountedGrossPrice": 180.0,
                    "discountRate": 0.1,
                    "cumulatedDiscountRate": 0.1,
                    "netPriceDiscountAmount": 18.18,
                    "grossPriceDiscountAmount": 20.0,
                    "vatDiscountAmount": 16.36,
                    "discountId": "0605770c-538a-49bf-997a-b08d4ed88a23",
                    "discountCode": "StaySubscribeddiscount",
                    "allDiscountsApplied": [
                        {
                            "grossPrice": 200.0,
                            "netPrice": 181.82,
                            "discountedGrossPrice": 180.0,
                            "discountedNetPrice": 163.64,
                            "discountRate": 0.1,
                            "vatDiscountAmount": 16.36,
                            "netPriceDiscountAmount": 18.18,
                            "grossPriceDiscountAmount": 20.0,
                            "discountId": "0605770c-538a-49bf-997a-b08d4ed88a23",
                            "model": "COUPON"
                        }
                    ]
                },
                "vatIncluded": true,
                "source": "INTERNAL"
            },
            "renewingPrice": {
                "vatAmount": 18.18,
                "netPrice": 181.82,
                "grossPrice": 200.0,
                "vatRate": 0.1,
                "currency": "AUD",
                "vatIncluded": true,
                "source": "INTERNAL"
            },
            "taxExempt": false
        }
    ]
}
```
To monitor accepted offers, subscribe to [Order Notifications](../notifications/05-orderNotification.md) with the event `type = created` and `source = offer`. This event notifies you when an order is created after a shopper accepts the offer. Shopper may also choose to decline the offer and cancel auto-renewal, the discount will not be applied than.

If shoppers cancel auto-renewal after accepting the offer, the discount will be removed from the renewal price. To track these changes, subscribe to Offer Notifications with the event `type = aborted`. This ensures the system removes the discount order if the shopper changes their decision. 


## Mid-Term Upgrade

The Mid-Term Upgrade flow enables shoppers to enhance their subscription by upgrading to a higher-tier plan or extending the number of devices. This feature applies only to subscriptions in their mid-cycle, before the Prebilling reminder has been sent. It also applies to subscriptions with auto-renewal disabled, in which case the Mid-Term upgrade will re-enable auto-renewal alongside the upgrade.

Nexway offers a one-click payment solution, enabling shoppers to use their existing payment method without needing to re-enter their details. Alternatively, they can provide a new payment method, which will be used for the upgrade and subsequent renewals of the upgraded subscription. Shoppers are charged a prorated amount, ensuring they pay only for the upgraded service for the remaining duration of the current billing cycle.

The upgrade takes effect immediately, updating the product while keeping the subscription term length unchanged.

### Part 1: Create a Mid-Term Upgrade Cart 

After shopper accepts for mid-term upgrade of their subscription, send a request to the Nexway API to create a shopping cart. Include the subscription identifier and details of the product the subscription is being upgraded to. The new product price must exceed the current subscription price.

**API Request Example**
```json
POST /carts/mid-term-upgrade
{
  "subscriptionId": "46d9841b-6e60-4479-9699-511c6d0f6ca2",
  "wantedProduct" : {
    "id": "8c7115e2-6fa2-454e-be09-50dd20ecacc4"
  }
}
```
The `201 Created` response includes product details and prorated price in `cart.price.discountedPrice`. 

**API Response Example**
```json
{
    "id": "0eb27503-674b-4855-b3bb-00f8386b64ea",
    "customerId": "27213244-fa24-4728-ae03-a42376aadd42",
    "enduserId": "417e7503-9bd9-48e8-a526-7b3add6f32b2",
    "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
    "products": [
        {
            "publisherRefId": "123",
            "id": "8c7115e2-6fa2-454e-be09-50dd20ecacc4",
            "customerId": "27213244-fa24-4728-ae03-a42376aadd42",
            "quantityMaxReached": false,
            "price": {
                "currency": "AUD",
                "netPrice": 40.91,
                "grossPrice": 45.0,
                "vatIncluded": true,
                "vatRate": 0.1,
                "vatAmount": 4.09,
                "discountedPrice": {
                    "discountedNetPrice": 4.59,
                    "discountedGrossPrice": 5.05,
                    "netPriceDiscountAmount": 36.32,
                    "grossPriceDiscountAmount": 39.95,
                    "vatDiscountAmount": 0.46,
                    "signedDiscount": "eyJjdXN0b21lcklkIjoiMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQyIiwicHJvZHVjdElkcyI6WyI4YzcxMTVlMi02ZmEyLTQ1NGUtYmUwOS01MGRkMjBlY2FjYzQiXSwic3RvcmVJZHMiOlsiNTk0MDk0ODItOTcxOS00ZDc2LTk3YWQtYzY3OWFjYzdkMTRhIl0sImVuZERhdGUiOjE3MzQwMDQ4MjQ3NTEsImFtb3VudEJ5Q3VycmVuY3kiOnsiQVVEIjozOS45NX19LS0tLU1Dd0NGQ1pwcE12OGlJdk1CN3NBTytkVmhKdkoxaS9xQWhRbi85VjQ2cFYzZXdJWHB6L1MxYkdURllrcjBRPT0tLS0tMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQy"
                },
                "source": "INTERNAL"
            },
            "defaultCurrency": "BRL",
            "unitPrice": {
                "currency": "AUD",
                "netPrice": 40.91,
                "grossPrice": 45.0,
                "vatIncluded": true,
                "vatRate": 0.1,
                "vatAmount": 4.09,
                "discountedPrice": {
                    "discountedNetPrice": 4.59,
                    "discountedGrossPrice": 5.05,
                    "netPriceDiscountAmount": 36.32,
                    "grossPriceDiscountAmount": 39.95,
                    "vatDiscountAmount": 0.46,
                    "signedDiscount": "eyJjdXN0b21lcklkIjoiMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQyIiwicHJvZHVjdElkcyI6WyI4YzcxMTVlMi02ZmEyLTQ1NGUtYmUwOS01MGRkMjBlY2FjYzQiXSwic3RvcmVJZHMiOlsiNTk0MDk0ODItOTcxOS00ZDc2LTk3YWQtYzY3OWFjYzdkMTRhIl0sImVuZERhdGUiOjE3MzQwMDQ4MjQ3NTEsImFtb3VudEJ5Q3VycmVuY3kiOnsiQVVEIjozOS45NX19LS0tLU1Dd0NGQ1pwcE12OGlJdk1CN3NBTytkVmhKdkoxaS9xQWhRbi85VjQ2cFYzZXdJWHB6L1MxYkdURllrcjBRPT0tLS0tMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQy"
                },
                "source": "INTERNAL"
            },
            "renewingPrice": {
                "currency": "AUD",
                "netPrice": 40.91,
                "grossPrice": 45.0,
                "vatIncluded": true,
                "vatRate": 0.1,
                "vatAmount": 4.09,
                "source": "INTERNAL"
            },
            "type": "SOFTWARE",
            "businessSegment": "B2C",
            "fullPrice": {
                "currency": "AUD",
                "netPrice": 40.91,
                "grossPrice": 45.0,
                "vatIncluded": true,
                "vatRate": 0.1,
                "vatAmount": 4.09,
                "discountedPrice": {
                    "discountedNetPrice": 4.59,
                    "discountedGrossPrice": 5.05,
                    "netPriceDiscountAmount": 36.32,
                    "grossPriceDiscountAmount": 39.95,
                    "vatDiscountAmount": 0.46,
                    "signedDiscount": "eyJjdXN0b21lcklkIjoiMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQyIiwicHJvZHVjdElkcyI6WyI4YzcxMTVlMi02ZmEyLTQ1NGUtYmUwOS01MGRkMjBlY2FjYzQiXSwic3RvcmVJZHMiOlsiNTk0MDk0ODItOTcxOS00ZDc2LTk3YWQtYzY3OWFjYzdkMTRhIl0sImVuZERhdGUiOjE3MzQwMDQ4MjQ3NTEsImFtb3VudEJ5Q3VycmVuY3kiOnsiQVVEIjozOS45NX19LS0tLU1Dd0NGQ1pwcE12OGlJdk1CN3NBTytkVmhKdkoxaS9xQWhRbi85VjQ2cFYzZXdJWHB6L1MxYkdURllrcjBRPT0tLS0tMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQy"
                }
            },
            "lifeTime": "1YEAR",
            "licenseNextExpirationDate": 1765536344000,
            "subscriptionTemplate": "NEXWAY_1Y",
            "fulfillmentTemplate": "42c365e5-32c2-47a9-87eb-a6ce09086762",
            "trial": false,
            "taxExempt": false,
            "renewingProductDetails": {
                "lifeTime": "1YEAR"
            },
            "previousLineItemId": "2f3d2c41-af75-458c-b881-d76eb19526d6",
            "quantity": 1,
            "name": "Product for upgrade",
            "priceSource": "INTERNAL",
            "subscriptionProduct": true,
            "paidTrial": false,
            "productFamily": "",
            "discountsStatus": [
                {
                    "discount": "eyJjdXN0b21lcklkIjoiMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQyIiwicHJvZHVjdElkcyI6WyI4YzcxMTVlMi02ZmEyLTQ1NGUtYmUwOS01MGRkMjBlY2FjYzQiXSwic3RvcmVJZHMiOlsiNTk0MDk0ODItOTcxOS00ZDc2LTk3YWQtYzY3OWFjYzdkMTRhIl0sImVuZERhdGUiOjE3MzQwMDQ4MjQ3NTEsImFtb3VudEJ5Q3VycmVuY3kiOnsiQVVEIjozOS45NX19LS0tLU1Dd0NGQ1pwcE12OGlJdk1CN3NBTytkVmhKdkoxaS9xQWhRbi85VjQ2cFYzZXdJWHB6L1MxYkdURllrcjBRPT0tLS0tMjcyMTMyNDQtZmEyNC00NzI4LWFlMDMtYTQyMzc2YWFkZDQy",
                    "status": "APPLIED"
                }
            ],
            "salesMode": "STANDARD",
            "fulfillmentTemplateName": "wiremock",
            "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
            "nextGenerationOf": [],
            "catalogId": "aa720d46-840a-418c-a0b5-b5356f31315b",
            "descriptionId": "99f22922-17db-4c0b-bfa6-065166dcf312",
            "trialAllowed": false,
            "unifiedRegisterSoftware": false,
            "physical": false,
            "expirationDate": 1765537224050
        }
    ],
    "locale": "en-US",
    "country": "AU",
    "endUser": {
        "email": "userEmail@domain.com",
        "maskedEmail": false,
        "lastName": "lastName",
        "firstName": "firstName",
        "city": "city",
        "zipCode": "51034",
        "country": "AU",
        "locale": "en-US",
        "taxExemptionEligible": false,
        "id": "417e7503-9bd9-48e8-a526-7b3add6f32b2",
        "customerId": "27213244-fa24-4728-ae03-a42376aadd42",
        "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
        "storeRoute": {
            "hostname": "testdomain.nexway.build",
            "fullUrl": "https://testdomain.nexway.build",
            "builtHostname": "testdomain.nexway.build"
        },
        "wallet": {
            "creditCards": [
                {
                    "id": "c1a30e53-9485-44f2-b8b0-7a28d3b0a38a",
                    "paymentMethodType": "CreditCard",
                    "type": "visa",
                    "bin": "411111",
                    "expirationDate": "01/2032"
                }
            ]
        },
            "paypals": []
        },
        "type": "BUYER"
    },
    "price": {
        "currency": "AUD",
        "netPrice": 40.91,
        "grossPrice": 45.0,
        "vatAmount": 4.09,
        "discountedPrice": {
            "discountedNetPrice": 4.59,
            "discountedGrossPrice": 5.05,
            "netPriceDiscountAmount": 36.32,
            "grossPriceDiscountAmount": 39.95,
            "vatDiscountAmount": 0.46
        }
    },
    "subsidiaryId": "1",
    "storeHostname": "testdomain.nexway.build",
    "checkoutUrl": "https://testdomain.nexway.build/checkout/add?cartId=0eb27503-674b-4855-b3bb-00f8386b64ea",
    "subscriptionId": "46d9841b-6e60-4479-9699-511c6d0f6ca2",
    "source": "MID_TERM_UPGRADE",
    "totalAmount": 5.05
}
```

### Part 2. Initiate Checkout
Use the checkoutUrl, endUserId, and storeId from the Create a Mid-Term Upgrade Cart response to create an authorized checkout. This allows the display of payment methods from the shopper's Wallet and auto-prefilling of their billing details.

**API Request Example**
```json
POST /iam/deeplink/
{
    "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
    "enduserId": "417e7503-9bd9-48e8-a526-7b3add6f32b2",
    "baseLink": "https://testdomain.nexway.build/checkout/selfrenew",
    "expiration_time": 1000,
    "single_use": false
}
```

The `Location` header in the `201 Created` response contains the authorized checkout URL.

**API Response Example**
```
Headers
Location: https://testdomain.nexway.build/checkout/selfrenew?deeplinkid=30fc528f-b8a9-4f67-b4c8-b428227aae78
```

Add the cart identifier received in the Create a Mid-Term Upgrade Cart response to the authorized checkout URL and redirect the shopper to the resulting link as in the example below.
```
https://testdomain.nexway.build/checkout/selfrenew?deeplinkid=30fc528f-b8a9-4f67-b4c8-b428227aae78&cartid=0eb27503-674b-4855-b3bb-00f8386b64ea
```

On the checkout page, the shopper reviews the order, confirms billing details, and selects a payment method (either from their wallet or by adding a new method).

Once the shopper confirms the order, Nexway converts the checkout into an order with the `source = MID_TERM_UPGRADE` and processes the payment. If the subscription’s auto-renewal was disabled, it will be re-enabled. Upon successful order completion, the shopper is redirected to a Thank you page and receives an email with order details.

To track the lifecycle of the Mid-Term upgrade order, subscribe to [Order event notifications](../notifications/05-orderNotification.md).


## Subscription import

The "Subscription import" or "Retail to Subscription" flow encourages shoppers who purchase non-renewable products (in the retail channel) to convert them into auto-renewal subscriptions. This helps improve retention rates and keeps shoppers subscribed.

After purchasing a non-renewable product, shoppers is offered through your platform’s user interface to activate auto-renewal with a zero-cost setup and receive a discount on the renewal. After accepting the offer, the shopper is redirected to the Nexway shopping cart to provide payment details and confirm the zero-price order. As a result of scenario the product’s validity remains unchanged, while auto-renewal is enabled. The discount applied during checkout will automatically take effect on the renewal.

**Key Details**

* The zero-price setup can be achieved by either integrating with your platform or by configuring it directly in Nexway through the Marketing Operations functionality
* To apply a renewal discount, set up a [discount](../guides/30-discount-api_guide.md) using either the Campaign model or the Subscription discount plan model
* The expiration date of the current product is determined and provided by your platform, either through integrating with your platform or via API inputs when creating a shopping cart, as outlined below.

### Part 1: Create a shopping cart 

You can create a shopping cart for the "Retail to Subscription" flow in to ways:

1. Using the API: This method provides greater flexibility, including the ability to pass the current expiration date and other advanced configurations

2. Using a Buy-Link: This method comes with some limitations, such as inability to pre-fill billing address and the current expiration date.

## Option 1: Using the Cart API

Send a request to create a shopping cart with the following key attributes to enable the "Retail to Subscription" flow:
* `wantedProducts.id`: The targeted product for the transition
* `wantedProducts.currentExpirationDate`: The expiration date of the current product
* `scenario = subscriptionimport`: Specifies the "Retail to Subscription" shopping cart.

**Optional Parameters**
* `marketingCampaignNames`: Include this if you’re using Marketing Operations to ensure the zero-price configuration
* `discountPlan`: Provide these attributes to apply a Subscription discount plan for multiple renewals
Pass billing information via API to streamline the checkout process for shoppers.

**API Request Example**
```json
POST /carts
{
    "storeId": "59409482-9719-4d76-97ad-c679acc7d14a",
     "endUser": {
        "email": "email@domain.com",
        "lastName": "lastName",
        "firstName": "firstName,
        "streetAddress": "street",
        "city": "city",
        "zipCode": "12345",
        "country": "FR",
        "locale": "fr-FR",
        "maskedEmail": true
    },
    "wantedProducts": [
        {
        "id": "2f9bb37b-3558-49f0-bea6-69ab834013de",
        "currentExpirationDate": "1713268338000
        }
    ],
    "discountPlan": { 
    "tag": "testDiscountPlan", 
    "discountStep": 0
    },
    "marketingCampaignNames": [
    "testCampaign"
  ],
    "externalContext": "eyd5b3VyQ29udGV4dEtleTEnOidzb21lLXZhbHVlLXlvdS13YW50LXRvLXJlY2VpdmUtYWZ0ZXItdGhlLXB1cmNoYXNlJ30=",
    "country": "FR",
    "currency": "EUR",
    "locale": "fr-FR",
    "scenario": "subscriptionimport"
}      
```
The `201 Created` response includes the `cartId` in the `Location` header.

**API Response Example**
```
Headers
Location: /carts/d513f26a-e36a-4b5d-ab7f-887de69bc21e
```
To retrieve the cart content, use the `cartId` with the API request below.

**API Request Example**
```
GET /carts/d513f26a-e36a-4b5d-ab7f-887de69bc21e
```
The `201 Created` response contains the `checkoutUrl` attribute. Use this URL to redirect the end-user to the shopping cart. 

## Option 2: Using a buy-link

Build a buy-link that directs the shopper straight to the shopping cart for review and checkout. 

**Buy-link Example**
```
https://storeName.mydomain.fr/checkout/add?products=2f9bb37b-3558-49f0-bea6-69ab834013de&mktop=testCampaign&scenario=subscriptionimport&&discountTag=testDiscountPlan&discountStep=0&theme=theme&layout=layout
```

### Part 2: Checkout

On the checkout page, the total price is displayed as zero, meaning the shopper does not have to pay immediately. However, shoppers are required to enter payment method details to be used during subscription renewals. If billing address was provided during the cart creation process, it will be pre-filled on the checkout page.

After the shopper confirms the order, Nexway processes the checkout and converts it into an order. As part of this process, the subscription is created. Once the order is successfully completed, the shopper is redirected to a Thank You page and receives a confirmation email. On the scheduled renewal date, the shopper will be charged with the applied discount, and the subscription will renew automatically for the next period.


## Updating Subscription Renewal Product

You can update the product associated with a subscription's renewal, allowing for upgrades or downgrades. 

:::note
 Changes made through this process will take effect at the next subscription renewal. The change can be done only before the prebilling of the subscription.
:::

:::note
 The target product must have the same term as the current subscription product.
:::


```json
POST /purchases/{subscriptionId}/

{
  "productId": "downgradedProductId"
}
```

**Input Parameter**

`productid` (Required, string): Product identifier.

**Response Codes**

- 200: Subscription renewal product is updated.
- 400: Request validation failure (e.g., subscription suspended, prebilling order exists, invalid product ID).
- 401: Authentication failed.
- 403: Forbidden (user not entitled to call the service).
- 404: Requested subscription not found.
- 500: Internal server error.