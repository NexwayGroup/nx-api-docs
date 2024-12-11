# Subscription management
The subscription scenarios outlined below are designed to enhance the user experience, boost shopper retention, and provide greater flexibility in managing subscription options.

## Stay Subscribed Offer
The Stay Subscribed Offer flow enables you to offer a discount on the upcoming subscription renewal to shoppers who wish to cancel their subscription's auto-renewal. This offer cannot be applied to subscriptions already in the renewal billing process or those in the trial period. When shoppers accept the offer, the discount is applied to their subscription renewal price, which will be charged during the auto-renewal process. Shoppers do not need to pay immediately when they accept the offer.

To use this feature, a [discount](30-discount-api_guide.md) with `source = OFFER` and `offerSubSource = SUSPEND` must first be configured in Nexway. Please note that the discount for “Stay Subscribed” flow can only be defined as a percentage value, not as an absolute amount.

## Integration options
Nexway provides two integration options to suit your business needs:

***Option 1: Using the Nexway-hosted End-User portal***


This is the simplest integration method. Nexway’s portal presents the Stay Subscribed offer directly to shoppers. Your platform does not need to be involved in the offer process. When shopperы log into their accounts on the Nexway End-User portal and attempt to cancel auto-renewal, they will see the offer. If they accept it, the discount is applied to their upcoming subscription renewal, and an order is created in the Nexway system. At the time of auto-renewal, the created order will be processed with the applied discount.


***Option 2: Integrating Nexway API***

This option allows full control through your platform’s user interface. 

**Part 1. Create a Stay Subscribed Offer**

When a shopper selects the option to cancel auto-renewal, your platform sends a request to the Nexway API to create the Stay Subscribed offer. This request includes the Nexway subscription identifier and, optionally, the [discount](30-discount-api_guide.md) identifier if you wish to explicitly specify the discount. 

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
            "name": "TEst Product",
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
To monitor accepted offers, subscribe to [Order Notifications](05-orderNotification.md) with the event type `created` and source `offer`. This event notifies you when an order is created after a shopper accepts the offer. Shopper may also choose to decline the offer and cancel auto-renewal, the discount will not be applied than.

If shoppers cancel auto-renewal after accepting the offer, the discount will be removed from the renewal price. To track these changes, subscribe to Offer Notifications with the event type `aborted`. This ensures the system removes the discount order if the shopper changes their decision. 