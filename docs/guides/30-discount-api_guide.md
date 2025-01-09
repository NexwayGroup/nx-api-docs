# Discount API guide
Discount API allows you to manage discount rules.

The service supports the following types of discount rules:
* Campaign (it's important to distinguish it from Marketing Campaigns, as they are different entities)
    * get discounts/&#123;id&#125; 
    * put discounts/&#123;id&#125;
    * post discounts/
    * delete discounts/&#123;id&#125;
* Coupon: reusable coupon code
* Single use coupon: should be generated with a special endpoint post /discounts/&#123;id&#125;/generate

## Campaigns

Campaigns allow to define discount rules with various options. These rules can be applied at specific points in the user journey:

* Acquisition: Attract new customers
* Trial to Paid Conversion: Encourage trial users to subscribe
* Subscription Renewal: Retain existing customers

**Automatic Application based on Eligibility**

Once configured with eligibility criteria, campaigns are automatically applied during relevant user actions. You don't need to specify discountId in each cart creation request.

Matching cart attributes with discount rule filters triggers its application. The filters are:
- Products -> "productIds": ```["d0a016b3-7620-4d0f-bb10-651f3929329d", "1064edd3-6bf3-4766-a242-ff7b4c853f4f"]```
- Parent products -> ```"parentProductIds": ["dcc37dde-caa6-45e4-bf7d-1729e55f9879"]```
- Product references ->  ```"publisherRefIds": ["11111111", "testcopyatca"]```
- Stores -> "storeIds": ```["c838c437-163e-470f-9f80-6cc969b10756", "1258522f-3640-4595-acbf-319b359868e0"]```
- Countires -> "countries": ```["AF", "AG"]```
- End-users email(s) -> ```"endUserEmails": ["user134@mail.com", "user234@mail.com"]```
- End-user type (Buyer or/and Reseller) -> endUserTypes": ```["RESELLER","BUYER"]```
- End-user group (Group has to be created first) ->  ```"endUserGroupIds": ["68f724f6-faa1-473a-8ba4-49aab287d879"]```
- Specific end-user (It has to be created first) -> ```"enduserId": "70225803-5593-46df-9af9-e68d773724cf",```
- Minumal cart amount (depending on currency) -> ```"thresholds": {"AED": 100}```
- Minimum/Maximum Order Value

**Multiple campaigns match**: When searching for campaigns, if multiple are found, the most recently updated one will be used. Combining multiple campaign-based discounts within a cart is not currently supported.

**Application target**: The "level" attribute ("CART" or "PRODUCT") defines where the discount applies: entire cart or individual line items.

**Tax dependency**: The "applyOnNetPrice" attribute (default: false) determines if the discount applies to the gross or net price (currently defaults to gross).

### Capping and limits
It is possible to additionally limit the discount rule by:
- start date / end date and time zone for it if needed: ```"startDate": "2016-01-01T00:00:00Z", "endDate": "2030-01-01T00:00:00Z",```
- total maximum uses of a discount: ```"maxUsages": 1,```
- maximum uses per store: ```"maxUsePerStore": 2,```
- maximum uses per end-user: ```"maxUsePerEndUser": 3,```

### Test order flag
By specifying test flag you can create test orders which will be automatically cancelled in few days.

### Example of discount rule

```json
{
  "model": "CAMPAIGN",
  "id": "bfbd4e75-d692-4655-b318-7dda978e867b",
  "customerId": "Nexway",
  "enduserId": "70225803-5593-46df-9af9-e68d773724cf",
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
    "PURCHASE"
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
  "endUserEmails": ["sb1@gmail.com", "test@gmail.com"],
  "weight": 0,
  "maxUsePerStore": 2,
  "maxUsePerEndUser": 3,
  "cumulative": false
}
```

### Campaign discount use cases ###

Let's review some common use cases for 'CAMPAIGN' discount rules.

#### Acquisition discount ####
- Create discount rule with model `CAMPAIGN`, set eligibilites and capping & limits as you need
- Set required discount value
- Set `sources = PURCHASE`
- Set level `level = PRODUCT`

This discount will be applied for each product in the cart:

```json 
{
    "model": "CAMPAIGN",
    "id": "3ce07e83-4902-4d31-b86e-7c5a2800d5d0",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
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
}
```

#### Discount conversion from trial to full subscription price ####
- Create a discount rule with model `CAMPAIGN`
- Set needed eligibility, capping and limits
- Set required discount value
- Set `sources = [ "SUBSCRIPTION" ]` and `subscriptionSubSources = "TRIAL_CONVERSION"`

This discount will be applied at the moment of conversion from trial to full subscription.

```json
{
    "model": "CAMPAIGN",
    "id": "be9ed61a-8758-4b81-a6bb-e6822433733f",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
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
}
```

#### Discount for subscription renewals ####
- Create a discount rule with model `CAMPAIGN`
- Set needed eligibility, capping and limits
- Set required discount value
- Set up `sources = [ "SUBSCRIPTION" ]` and `subscriptionSubSources = [ "RENEWAL" ]`

This discount will be applied at each subscription renewal.

```json
{
    "model": "CAMPAIGN",
    "id": "f2b4ae46-b65d-44e8-b1f3-a77d749a380d",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
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
}
```

#### Set up discounts for different subscription renewal generations ####
This scenario is almost the same as the discount for subscription renew, but with extra ability to apply different discount for each renewal generation. It is possible to specify discount for the first and the second renewal only, OR for the second and the fifth. Number of possible discounted renewal generations is limited to 10.


This discount will be applied at 1st, 3rd, 4th and 10th renewal
```json
{
  "model": "CAMPAIGN",
  "id": "bfbd4e75-d692-4655-b318-7dda978e867b",
  "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
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
}
```

#### Set up a discount for "Stay Subscribed" offer ####

The "Stay Subscribed" offer appears at the cancellation point, aiming to **retain** users with active subscriptions. It incentivizes them to **continue** their subscription by offering a special discount on their next renewal **if they choose to stay subscribed** instead of cancelling.

To activate "Stay Subscribed" offer you need to create a discount with the `model = "CAMPAIGN"`, `source = "OFFER"`  and `offerSubSource = "SUSPEND"`. In this case the discount will be found automatically during the offer creation.

The highest priority for applying a discount is given to the product.

If you want to specify explicitly what discount should be applied for the offer, you need to create a discount with the `model = "COUPON"`, `source = "OFFER"` and `offerSubSource = "SUSPEND"`.

Example for stay subscribed campaign:

```json 
{
    "model": "CAMPAIGN",
    "id": "7146053f-fd0a-4cc3-9d04-8b042c957c37",
    "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
    "endDate": "2023-12-31T11:53:00Z",
    "storeIds": [
        "36f48867-d6ca-42d3-bf55-5f54a6740803"
    ],
    "sources": [
        "OFFER"
    ],
    "offerSubSource": "SUSPEND",
    "discountRate": 0.1,
    "applyOnNetPrice": false,
    "name": "Test discount offer",
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

Example for coupon:

```json
{
  "model": "COUPON",
  "id": "eac96d45-a392-4507-9966-d30617fbcb6b",
  "customerId": "82223530-f443-4c15-a901-b4a88f994ac7",
  "lastUpdateReason": "resource update thru REST Api",
  "endDate": "2027-01-02T07:48:00Z",
  "sources": [
    "OFFER"
  ],
  "offerSubSource": "SUSPEND",
  "testOrder": false,
  "discountRate": 0.3,
  "applyOnNetPrice": false,
  "name": "myTestStaySubscribedCoupon",
  "status": "ENABLED",
  "level": "PRODUCT",
  "endUserTypes": [
    "RESELLER",
    "BUYER"
  ],
  "weight": 0,
  "codes": {
    "STAY1": "default"
  },
  "cumulative": true
}

```

Expected end-user expirience:
1. End-user opens end-user portal to suspend his subscription
2. End-user initiates suspending
3. End-user portal displays discount offer for the next renewal in case if auto-renewal will be kept
4. End-user accepts the offer and keeps auto-renewal enabled


## Coupons Codes

Coupon codes allow you to offer targeted discounts to your customers.

The API supports two discount models for coupon codes:

- `COUPON`: This model represents a reusable coupon code. The same code can be used by multiple customers.
- `SINGLE_USE_CODE`: This model represents a unique coupon code. Each code can only be used once.

### Applying Coupon Codes:

There are two main ways coupon code can be applied:

- **Shopping Cart**: End-users can enter the coupon code into a designated field within the shopping cart interface
- **Buy Link**: You can create buy links with a `discounts` query parameter containing the coupon code. This allows users to redeem the discount automatically upon clicking the link

### Single Use Coupon Codes

In order to generate a batch of single use codes you need to create a discount rule first:

```json
{
    "customerId": "55555555-9999-4999-baba-777777777777",
    "discountRate": 0.5,
    "endDate": "2025-07-08T16:58:00Z",
    "productIds": ["77777777-ffff-49b0-9efa-777777777777"],
    "endUserTypes": [ "BUYER" ],
    "level": "PRODUCT",
    "maxUsages": "1",
    "model": "SINGLE_USE_CODE",
    "name": "50off-sample",
    "status": "ENABLED"
}
```

Once the discount rule is created you can generate a batch of coupon codes by calling the /generate method.
The following parameters can be specified in a request json body:

- `quantity`: there could be no more than 750 codes associated with one discount rule;
- `size`: the length of the coupon code. Default is 8;
- `prefix`: to start each coupon code with.

`POST https://api.nexway.store/discounts/&#123;id&#125;/generate`

```json
{
    "quantity": 200,
    "size": 10,
    "prefix": "xyz"
}
```


You'll receive the codes in the response:

```json
{
    "remaining": 0,
    "codes": ["ywI8XEy3"]
}
```

The "usage" endpoint will return the details of coupon usage associated with a discount rule: 

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
So `"used": true` means that coupon is already used, and you can see the `orderId` and `endUserEmail` of end-user who did it.

You can also sum up coupons usages by "recap" end point. "target" will filter with the "used" field: 

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

So `"count": 4` is the number of used coupons