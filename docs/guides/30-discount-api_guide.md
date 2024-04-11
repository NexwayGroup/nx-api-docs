# Discount API guide
Discount API allow you to manage discount rules.

Service allows to create different types of discount rules :
* Campaign (don't mess it with Marketing campaigns - its a different things): get discounts/{id} ; put discounts/{id} ; post discounts/ ; delete discounts/{id}
* Coupon: reusable coupon code
* Single use coupon: should be generated with a special endpoint post /discounts/{id}/generate

There is also a Marketing Campaing Operation Management fucntionality, which allows to override the price of product in cart by using specific marketing campaing identifier AND creates bundles, but it manages via different service, so its out of scope of this guide. To find more details about please revise 35-Marketing-campaigns-management-(MKTOP).md guide

## Campaigns usage

Campaign it's a discount rule which can help you to set different variation of discounts on different steps of end-user journey: at acquisition, at conversion from trial to full subscription, at subscription renew and so on.

The main thing about campaing is that it should be just configured with proper "eligibility" to be automatically added into cart / applied on subscription renew. So no need to specify discount attributes in cart creation request - you should just to set it up properly and Monetize will choose the most profittbale to end-user discount and apply it automatically at the required step.

### How to connect campaing with product in cart
The basisc level of connection is on customer level - so the customerId is a mandatatory parameter for any discount rule we're creating.
We can make connection more specific by specifing following additional attributes. All these attributes can be combined with each other and they work as as a filters to find corresponding discount rule for a exact cart.

- End-user type (Buyer or/and Reseller) -> endUserTypes": ```["RESELLER","BUYER"]```
- End-user group (Group has to be created first) ->  ```"endUserGroupIds": ["68f724f6-faa1-473a-8ba4-49aab287d879"]```
- Specific end-user (It has to be created first) -> ```"enduserId": "70225803-5593-46df-9af9-e68d773724cf",```

- Minumal cart amount (depending on currency) -> ```"thresholds": {"AED": 100}```

- Countires -> "countries": ```["AF", "AG"]```
- Stores -> "storeIds": ```["c838c437-163e-470f-9f80-6cc969b10756", "1258522f-3640-4595-acbf-319b359868e0"]```
- Products -> "productIds": ```["d0a016b3-7620-4d0f-bb10-651f3929329d", "1064edd3-6bf3-4766-a242-ff7b4c853f4f"]```
- Parent products -> ```"parentProductIds": ["dcc37dde-caa6-45e4-bf7d-1729e55f9879"]```
- Product references ->  ```"publisherRefIds": ["11111111", "testcopyatca"]```

So if product in cart matches with combination of conditions which are set up for discount rule - this discount rule will be applied to a cart.
If there are more than one discount rule found - Monetize will pick a rule with the biggest discount value. Its not possible for a moment to combine several discount rule with model "campaign" in one cart.

Its also possible to configure on what level the discount will be applied - for whole cart, or for each line item in cart. It manages via ```"level": "CART"``` attribute. Possible values are CART and PRODUCT. If the level is "CART" - discount rulle will be applied to total cart amount, if level is "PRODUCT" - discount rule will be applied to each lineitem in cart

We can manage on what price the discount will be applied - gross or net. It manages via ```"applyOnNetPrice": false``` attribute, which is false by default.

### Capping and limits
Its possible to limit when discount rule will be apllied by setting following attributes:
- start date / end date and time zone for it if needed: ```"startDate": "2016-01-01T00:00:00Z", "endDate": "2030-01-01T00:00:00Z",```
- total maximum uses of a discount: ```"maxUsages": 1,```
- maximum uses per store: ```"maxUsePerStore": 2,```
- maximum uses per end-user: ```"maxUsePerEndUser": 3,```


### Test order flag
This feature allows you to create a test order with discount on production env. Such an order will be automatically closed in a 30 days after order's creation. This can be configured 

<strong>Example</strong>:

```json
{
  "model": "CAMPAIGN",
  "id": "bfbd4e75-d692-4655-b318-7dda978e867b",
  "customerId": "Nexway",
  "enduserId": "70225803-5593-46df-9af9-e68d773724cf",
  "createDate": 1657643597149,
  "updateDate": 1712742442854,
  "dbVersion": 6,
  "lastUpdateReason": "resource update thru REST Api",
  "startDate": "2016-01-01T00:00:00Z",
  "endDate": "2030-01-01T00:00:00Z",
  "storeIds": [
    "c838c437-163e-470f-9f80-6cc969b10756",
    "1258522f-3640-4595-acbf-319b359868e0"
  ],
  "productIds": [
    "d0a016b3-7620-4d0f-bb10-651f3929329d",
    "1064edd3-6bf3-4766-a242-ff7b4c853f4f"
  ],
  "parentProductIds": [
    "dcc37dde-caa6-45e4-bf7d-1729e55f9879"
  ],
  "publisherRefIds": [
    "11111111",
    "testcopyatca"
  ],
  "sources": [
    "MANUAL_RENEWAL"
  ],
  "testOrder": false,
  "discountRate": 0.2,
  "applyOnNetPrice": false,
  "name": "name2862",
  "countries": [
    "AF",
    "AG"
  ],
  "status": "ENABLED",
  "maxUsages": 1,
  "level": "CART",
  "thresholds": {
    "AED": 100
  },
  "endUserTypes": [
    "RESELLER",
    "BUYER"
  ],
  "endUserGroupIds": [
    "68f724f6-faa1-473a-8ba4-49aab287d879"
  ],
  "weight": 0,
  "maxUsePerStore": 2,
  "maxUsePerEndUser": 3,
  "cumulative": false
}
```

### Campaign discount use cases

Let's consider the most common use cases of discount rules with model "campaing"

### Acquisition discount ####
- Create discount rule with model "Campaign", set eligibilites and capping & limits as you need
- Set required discount value
- Set up sources = purchase

<strong>Example</strong>:

```json 
{
    "model": "CAMPAIGN",
    "id": "3ce07e83-4902-4d31-b86e-7c5a2800d5d0",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1602226213085,
    "updateDate": 1712827017195,
    "dbVersion": 20,
    "lastUpdateReason": "Nexway-Center PUT : reason not specified",
    "endDate": "2025-02-11T21:59:59Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "productIds": [],
    "sources": [
        "PURCHASE"
    ],
    "testOrder": false,
    "discountRate": 0.1,
    "applyOnNetPrice": false,
    "name": "IAP8544",
    "localizedLabels": {},
    "countries": [],
    "status": "ENABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}
```

**As the result this discount will be applied in the cart for each product for a given conditions**

#### Discount conversion from trial to full subscription price ####
- Create discount rule with model "Campaign", set eligibilites and capping & limits as you need
- Set required discount value
- Set up sources = subscription AND subscriptionSubSources = trial_conversion

```json
{
    "model": "CAMPAIGN",
    "id": "be9ed61a-8758-4b81-a6bb-e6822433733f",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1631542438421,
    "updateDate": 1633441363544,
    "dbVersion": 8,
    "lastUpdateReason": "Nexway-Center PUT : C.R.U.D. Helper operation",
    "endDate": "2021-10-31T22:59:59Z",
    "productIds": [
        "a88aa33d-90ce-4942-8fb5-5db08efba384"
    ],
    "sources": [
        "SUBSCRIPTION"
    ],
    "subscriptionId": "a2d2f8bd-789f-4fe8-bfaf-62b58a73e3e4",
    "subscriptionSubSources": [
        "TRIAL_CONVERSION"
    ],
    "testOrder": false,
    "discountRate": 0.5,
    "applyOnNetPrice": false,
    "name": "trial_conversion_test",
    "localizedLabels": {
        "neutral": "trial_conversion_test"
    },
    "status": "ENABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}
```

**As the result this discount will be applied at the moment of conversion from trial to full subscription. Its also possible to display it in the cart renewingPrice**

#### Discount of subscription renewals ####
- Create discount rule with model "Campaign", set eligibilites and capping & limits as you need
- Set required discount value
- Set up sources = subscription AND subscriptionSubSources = renew

```json
{
    "model": "CAMPAIGN",
    "id": "f2b4ae46-b65d-44e8-b1f3-a77d749a380d",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1650283181576,
    "updateDate": 1650283181576,
    "dbVersion": 0,
    "lastUpdateReason": "Nexway-Center POST : reason not specified",
    "endDate": "2022-04-25T20:59:59Z",
    "productIds": [
        "6e07ce95-34f6-4e52-bbe1-f68eab1c9f91",
        "0c1b53f6-554a-4089-9df5-35f2d28e4f00",
        "9650828b-67bc-4210-83e7-35455387bd11"
    ],
    "sources": [
           "SUBSCRIPTION"
    ],
    "subscriptionSubSources": [
        "RENEWAL"
    ],
    "testOrder": false,
    "discountRate": 0.2,
    "applyOnNetPrice": false,
    "name": "20% On renewal",
    "localizedLabels": {
        "neutral": "20% off"
    },
    "status": "ENABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}
```

**As the result this discount will be applied at each subscription renewal. Its also possible to see it in a cart renewingPrices**

#### Set up discount per subscription generation renew ####
Almost the same as a discount for subscription renew, but with extra ability to specify on what renew this discount will be applied. F.e. its possible to specify discount applying on 1st and 2nd renew only, or on 2nd and 5th. The limit of renewal count is 10.

```json
{
{
  "model": "CAMPAIGN",
  "id": "bfbd4e75-d692-4655-b318-7dda978e867b",
  "customerId": "Nexway",
  "enduserId": "70225803-5593-46df-9af9-e68d773724cf",
  "createDate": 1657643597149,
  "updateDate": 1712837768739,
  "dbVersion": 8,
  "lastUpdateReason": "resource update thru REST Api",
  "startDate": "2016-01-01T00:00:00Z",
  "endDate": "2030-01-01T00:00:00Z",
  "storeIds": [
    "c838c437-163e-470f-9f80-6cc969b10756",
    "1258522f-3640-4595-acbf-319b359868e0"
  ],
  "productIds": [
    "d0a016b3-7620-4d0f-bb10-651f3929329d",
    "1064edd3-6bf3-4766-a242-ff7b4c853f4f"
  ],
  "parentProductIds": [
    "dcc37dde-caa6-45e4-bf7d-1729e55f9879"
  ],
  "publisherRefIds": [
    "11111111",
    "testcopyatca"
  ],
  "sources": [
    "SUBSCRIPTION"
  ],
  "subscriptionGenerations": [
    1,
    3,
    4,
    10
  ],
  "subscriptionSubSources": [
    "RENEWAL"
  ],
  "testOrder": false,
  "discountRate": 0.2,
  "applyOnNetPrice": false,
  "name": "name2862",
  "countries": [
    "AF",
    "AG"
  ],
  "status": "ENABLED",
  "level": "Product",
  "endUserTypes": [
    "RESELLER",
    "BUYER"
  ],
  "endUserGroupIds": [
    "68f724f6-faa1-473a-8ba4-49aab287d879"
  ],
  "weight": 0,
  "cumulative": false
}
}
```
**As the result this discount will be applied at 1st, 3rd, 4th and 10th renewal**

#### Set up suspend offer discount for subscriptions ####

<strong>Setting up discounts</strong>

In the Stay Subscribed flow, only discounts with the model "CAMPAIGN" & source "OFFER" & subSource "SUSPEND" should be taken into consideration, while discounts of any other sources & model should be ignored.

The highest priority for applying a discount is given to the product.

Samples of discounts:

```json 
{
    "model": "CAMPAIGN",
    "id": "7146053f-fd0a-4cc3-9d04-8b042c957c37",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1690977177529,
    "updateDate": 1695279908905,
    "dbVersion": 3,
    "lastUpdateReason": "Nexway-Center PUT : reason not specified",
    "endDate": "2023-12-31T11:53:00Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "sources": [
        "OFFER"
    ],
    "offerSubSource": "SUSPEND",
    "testOrder": false,
    "discountRate": 0.1,
    "applyOnNetPrice": false,
    "name": "Test discount offer",
    "localizedLabels": {},
    "status": "ENABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}

{
    "model": "CAMPAIGN",
    "id": "14bf41b2-48c8-4149-8994-e87fb05e50a3",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1697526352090,
    "updateDate": 1698650566266,
    "dbVersion": 4,
    "lastUpdateReason": "Nexway-Center PUT : reason not specified",
    "endDate": "2023-10-24T20:59:59Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "parentProductIds": [
        "7f83cd51-7169-4eb5-b7a1-5f881456be22"
    ],
    "sources": [
        "OFFER"
    ],
    "offerSubSource": "SUSPEND",
    "testOrder": false,
    "discountRate": 0.09,
    "applyOnNetPrice": false,
    "name": "16652 discount offer suspend 9",
    "localizedLabels": {},
    "status": "DISABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}

{
    "model": "CAMPAIGN",
    "id": "547ff4f0-98a8-4941-8feb-1c7b6d206958",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1697526264604,
    "updateDate": 1698650629508,
    "dbVersion": 4,
    "lastUpdateReason": "Nexway-Center PUT : reason not specified",
    "endDate": "2023-10-24T20:59:59Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "publisherRefIds": [
        "KL1041FDAFF"
    ],
    "sources": [
        "OFFER"
    ],
    "offerSubSource": "SUSPEND",
    "testOrder": false,
    "discountRate": 0.14,
    "applyOnNetPrice": false,
    "name": "16652 discount offer suspend 14",
    "localizedLabels": {},
    "status": "DISABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}

{
    "model": "CAMPAIGN",
    "id": "e55546d0-c639-439d-ac39-04007a199585",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "createDate": 1694770687075,
    "updateDate": 1697803958238,
    "dbVersion": 15,
    "lastUpdateReason": "resource update thru REST Api",
    "endDate": "2024-09-26T20:59:59Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "productIds": [
        "c5a664ee-c2e1-47bf-bf8e-91b2ff335d8b"
    ],
    "parentProductIds": [],
    "publisherRefIds": [],
    "sources": [
        "OFFER"
    ],
    "offerSubSource": "SUSPEND",
    "testOrder": false,
    "discountRate": 0.05,
    "applyOnNetPrice": false,
    "name": "16652 discount offer suspend 5",
    "localizedLabels": {},
    "status": "ENABLED",
    "level": "PRODUCT",
    "endUserTypes": [
        "RESELLER",
        "BUYER"
    ],
    "endUserGroupIds": [],
    "weight": 0,
    "cumulative": false
}
```

<strong>Create subscription</strong>

On this step you have to select product with subscription and perform checkout for this product. After successful purchase you have to check if subscription is active  and order is completed.

<strong>Create offer</strong>

During offer creation the product from subscription will be selected and discount applied. The result will be a new cart with a renewal product with discounted price.

 Only one discount can be applied, if any. If there are many discounts, which may be applied, then current algorithm will take discount with the nearest update date (not the discount value!)  


There are 2 main cases for creating an offer.

- When prebilling order is created before creating stay subscribed offer.
- Price is taken from prebilling order and discount will be applied on it.
- When there is no prebilling order created before creating stay subscribed offer.
- Price is taken from product and discount will be applied on it.

<strong>Accept offer</strong>
User accepts offer by creating a prebilling order from a subscription renewal offer. You need cartId and subsctiptionId from previous steps.

Currently it’s implemented only for active subscriptions, next step is to implement it for trial and dunning ones.


6. Discount for manual renewal flow
TBD

## Coupons Usage

The "usage" endpoint will return you the detail of coupon usage of your discount rule : 

GET /discounts/usages?discountId=2a52b404-0bb0-4a14-99f7-7d1fe2717eb8
```json
{
  "items": [
    {
      "id": "0be1260c-cdf8-41ac-b874-11954bfc5d2d",
      "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
      "createDate": 1612872701654,
      "updateDate": 1612872701654,
      "dbVersion": 0,
      "lastUpdateReason": "notify usage of single codes thru REST Api",
      "discountId": "2a52b404-0bb0-4a14-99f7-7d1fe2717eb8",
      "discountCode": "leave",
      "orderId": "2H0ACTCJ9EE",
      "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
      "endUserEmail": "aaa@nexway.com",
      "useDate": 1612872701654,
      "used": true
    },
    {
      "id": "83e7d576-3ce3-4525-b150-1da07da0a862",
      "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
      "createDate": 1609855389687,
      "updateDate": 1609855389687,
      "dbVersion": 0,
      "lastUpdateReason": "notify usage of single codes thru REST Api",
      "discountId": "2a52b404-0bb0-4a14-99f7-7d1fe2717eb8",
      "discountCode": "leave",
      "orderId": "2FMEXCJYG5J",
      "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
      "endUserEmail": "xxx@nexway.com",
      "useDate": 1609855389686,
      "used": true
    },
    {
      "id": "44750a4f-f850-463d-b03c-d7c2c6cf6346",
      "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
      "createDate": 1609854293909,
      "updateDate": 1609854293909,
      "dbVersion": 0,
      "lastUpdateReason": "notify usage of single codes thru REST Api",
      "discountId": "2a52b404-0bb0-4a14-99f7-7d1fe2717eb8",
      "discountCode": "leave",
      "orderId": "2FME9V0HT0Y",
      "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
      "endUserEmail": "eee@nexway.com",
      "useDate": 1609854293909,
      "used": true
    },
    {
      "id": "a2fb2da7-6972-4ba4-816f-0acee568779d",
      "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
      "createDate": 1608041045277,
      "updateDate": 1608041045277,
      "dbVersion": 0,
      "lastUpdateReason": "notify usage of single codes thru REST Api",
      "discountId": "2a52b404-0bb0-4a14-99f7-7d1fe2717eb8",
      "discountCode": "leave",
      "orderId": "2EREPNDYWA1",
      "storeId": "36f48867-d6ca-42d3-bf55-5f54a6740803",
      "endUserEmail": "zzz@nexway.com",
      "useDate": 1608041045200,
      "used": true
    }
  ],
  "last": true,
  "totalItems": 4,
  "totalPages": 1,
  "size": 50,
  "number": 0
}
```

You can also sum up coupons usages by using the "recap" end point. "target" will filter with the "used" field : 

GET /discounts/usages/recap?discountId=2a52b404-0bb0-4a14-99f7-7d1fe2717eb8&target=used
```json
{
  "items": [
    {
      "targetValue": "null",
      "count": 4
    }
  ],
  "last": true,
  "totalItems": 1,
  "totalPages": 0,
  "size": 250,
  "number": 0
}
```

So count:4 is the number of used coupons

