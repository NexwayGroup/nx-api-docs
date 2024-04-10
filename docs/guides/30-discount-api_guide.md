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

- End-user type (Buyer or/and Reseller) -> endUserTypes": ```json["RESELLER","BUYER"]```
- End-user group (Group has to be created first) ->  ```json"endUserGroupIds": ["68f724f6-faa1-473a-8ba4-49aab287d879"]```
- Specific end-user (It has to be created first) -> ```json"enduserId": "70225803-5593-46df-9af9-e68d773724cf",```

- Minumal cart amount (depending on currency) -> ```json"thresholds": {"AED": 100}```

- Countires -> "countries": ```json["AF", "AG"]```
- Stores -> "storeIds": ```json["c838c437-163e-470f-9f80-6cc969b10756", "1258522f-3640-4595-acbf-319b359868e0"]```
- Products -> "productIds": ```json["d0a016b3-7620-4d0f-bb10-651f3929329d", "1064edd3-6bf3-4766-a242-ff7b4c853f4f"]```
- Parent products -> ```json"parentProductIds": ["dcc37dde-caa6-45e4-bf7d-1729e55f9879"]```
- Product references ->  ```json"publisherRefIds": ["11111111", "testcopyatca"]```

So if product in cart matches with combination of conditions which are set up for discount rule - this discount rule will be applied to a cart.
If there are more than one discount rule found - Monetize will pick a rule with the biggest discount value. Its not possible for a moment to combine several discount rule with model "campaign" in one cart.

Its also possible to configure on what level the discount will be applied - for whole cart, or for each line item in cart. It manages via ```json"level": "FORCED_CROSS_SELL"``` attribute.

We can manage on what price the discount will be applied - gross or net. It manages via ```json"applyOnNetPrice": false``` attribute, which is false by default.

### Capping and limits
Its possible to limit when discount rule will be apllied by setting following attributes:
- start date / end date and time zone for it if needed: ```json"startDate": "2016-01-01T00:00:00Z", "endDate": "2030-01-01T00:00:00Z",```
- total maximum uses of a discount: ```json"maxUsages": 1,```
- maximum uses per store: ```json"maxUsePerStore": 2,```
- maximum uses per end-user: ```json"maxUsePerEndUser": 3,```

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
  "level": "FORCED_CROSS_SELL",
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

1. Discount acquisition price of product

2. Discount renewal price of subscription

3. Discount conversion from trial to full subscription price

4. Set up discount per subscription generation renew

5. Set up suspend/resume offer discount for subscriptions 

6. Discount for manual renewal flow


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

