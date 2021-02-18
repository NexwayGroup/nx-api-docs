# Discount API guide
Discount API allow you to manage discount rules.

Service allows to create different types of discount rules :
* Campaign 
* Coupon : reusable coupon code
* Single use coupon : should be generated with a special endpoint post /discounts/{id}/generate

## Usage

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

