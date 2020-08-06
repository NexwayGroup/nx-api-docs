## Price API guide
Price service is a place to manage prices for products. Service allows to find or create prices for products. Prices once created cannot be deleted, unless they are scheduled in the future (their startDate is not reached yet).
## General
There are two ways to manage prices. 
Prices declared/updated through product creation/update with Product API in `prices` node are automatically fed into Price API.
POST `/products` - request body:
```json
{
  ...
  "prices": {
     "defaultCurrency": "USD",
     "priceByCountryByCurrency": {
        "USD": {
           "default": {
              "value": 100,
              "vatInclude": true
           }
        },
        "EUR": {
           "default": {
              "value": 200,
              "vatInclude": true
           },
           "FR": {
              "value": 300,
              "vatInclude": true
           },
           "DE": {
              "value": 400,
              "vatInclude": true
           }
        }
     }
  },
  ...
}
```
After product creation all prices are automatically created in Price API with validity dates from product creation date to infinity.


Second way to create prices is to make a request directly to Price API: 
POST `/prices` - request body: 
```json
{
  "customerId": "myCustomerId",
  "productId": "myProductId",
  "startDate": "2019-09-28T13:22:00Z",
  "endDate": "2019-11-28T13:22:00Z", (if not declared, endDate will be infinity)
  "country": "FR",
  "currency": "EUR",
  "msrp": 200.00,
  "value": 130.00,
  "vatIncluded": false
}
```
With using Price API directly, prices can be created, scheduled for the future (created with the startDate in the future), updated or deleted (only if they are scheduled in the future).

When creating a price in Price API those fields are required:
* customerId - Customer identifier (ex:  60f70f89-0498-487e-ba55-2cac045d4171)
* productId - Product identifier (ex:  60f70f89-0498-487e-ba55-2cac045d4171)
* startDate - Price start date timestamp in milliseconds (ex: 1589439239780)
* currency - Currency code in ISO-4217 format (ex. EUR)
* value - Regular price, two decimal places format (ex. 14.99)
* vatIncluded - boolean flag indicating if tax is included in price (true or false) 

and those are the additional fields:
* endDate - Price end date timestamp in milliseconds (ex: 1589439239780)
* country - Country code in ISO 3166-2 format (ex: FR) - restricts price to a specific country
* msrp - Manufacturer suggested retail price, two decimal places format (ex. 29.99)
* marketingCampaignId - Marketing campaign ID - restricts price only to a specific marketing campaign
* archived - Indicator if the price was removed/archived


## getBestPrice endpoint
Additional endpoint in Price API is used to get the most accurate price for given product for given parameters. This way you can search for a price that would be served in a cart for a product at specific date, for specific store's country or requested currency etc.

Parameters are passed as query params for the endpoint `/best`.
Required parameters:
* customerId
* productId
* country

Additional parameters:
* currency
* defaultCurrency (to fallback if no price found for currency above)
* marketingCampaignId
* date
If date not specified, price will be searched for current date

Logic behind price search based on provided parameters:

1. date

If date is not given in query param, price will be searched for current date. This is important because the price will be searched only within prices valid on a given date. So if we have timeline like this with only one price defined:
`......A......|...NoPrice...|...B......>`  
`A - startDate: 2020-01-01, endDate: 2020-10-31`  
`B - startDate: 2021-01-01, endDate: none`  
with a gap between 2020-10-31 and 2021-01-01, price request for dates within a gap will result with no price returned, as prices A and B will not be taken into account during the search. Keep in mind that having gap between prices is not a valid config and will indicate the wrong configuration of the product itself.

2. marketingCampaignId

If given, prices declared for marketingCampaignId will be considered a priority. If none declared or found for given product requested, regular prices (without marketingCampaignId set) will be searched.

3. currency

If given, only prices for this specific currency will be returned. Service will never return a price for a different currency than requested.

4. country

Required parameter. Price will be searched for given country. If none found, price will be searched for country's default currency.
For example, having prices declared for a product like this (presented as a product resource's node):
```
  ...
  "prices":{
     "defaultCurrency":"EUR",
     "priceByCountryByCurrency":{
        "EUR":{
           "default":{
              "value":2000,
              "msrp":3000,
              "vatInclude":true
           },
           "FR":{
              "value":1899,
              "msrp":2099,
              "vatInclude":true
           },
           "DE":{
              "value":899,
              "msrp":1099,
              "vatInclude":true
           }
        }
     }
  },
...
```
searching for a price with currency=EUR and country=FR, price returned will be 1899 EUR.
searching for a price with currency=EUR and country=DE, price returned will be 899 EUR.
searching for a price with currency=EUR and country=ES, price returned will be 2000 EUR - default ES currency is used.

5. defaultCurrency
 
If no prices found and defaultCurrency parameter is set, prices for that currency are searched. Having prices declared as in example above, searching for price with country=US and defaultCurrency=EUR, price returned will be 2000 EUR - default EUR price.


---
Sample response:
```json
{
   "id": "dabd72a8-e0df-4dfb-a206-6c280b72340a",
   "customerId": "1524e361-fee5-4422-bd45-29bf7f982809",
   "createDate": 1594109075604,
   "updateDate": 1594109075604,
   "dbVersion": 0,
   "lastUpdateReason": "Product service update",
   "productId": "7b042106-614c-4d1e-9247-c9f394215368",
   "startDate": 1594109074338,
   "country": "FR",
   "currency": "EUR",
   "value": 300.0,
   "vatIncluded": true,
   "archived": false,
   "history": [
       {
           "event": "CREATED",
           "when": 1594109075604
       }
   ]
}
```
If service doesn't find any prices, then return information: 
`Price not found for product "productId" for "given country" on date "given date"`
```json
{
   "timestamp": 1594109151537,
   "status": 404,
   "error": "Not Found",
   "message": "Price not found for product 7b042106-614c-4d1e-9247-c9f394215368 for CH on date 12 Oct 2030 00:00:00 GMT",
   "path": "/prices/best"
}
```

## Price creation and overlapping prices behavior
Creating a new price that overlaps with already existing prices will cause an adjustments of existing price so there are no overlaps in the price timeline.

Case 1.
```
given price A
....................Price A......................>
adding price B
                       |........Price B..........>
....................Price A......................>
result
........Price A........|........Price B..........>
```
Having a product with one price A declared:
`A - startDate: 2020-03-01, endDate: infinity`
Creating a new price B with the same parameters as price A:  
`B - startDate: 2020-10-01, endDate: infinity`
as a result we will get:
`A - startDate: 2020-03-01, endDate: 2020-09-30`
`B - startDate: 2020-10-01, endDate: infinity`

Case 2.
```
given price A
.......................Price A........................>
adding price B
                 |....Price B....|
....................Price A......................>
result
.....Price A.....|....Price B....|.....Price C........>
```
Having a product with one price A:  
`A - startDate: 2020-03-01, endDate: infinity`
Creating a new price B with the same parameters as price A:
`B - startDate: 2020-10-01, endDate: 2021-01-31`
as a result we will get:
`A - startDate: 2020-03-01, endDate: 2020-09-30`
`B - startDate: 2020-10-01, endDate: 2021-01-31`
`C - startDate: 2021-02-01, endDate: infinity`
Price C was created by Price API (because price B interrupts the continuity of duration of Price A) and has the same parameters and value as Price A.

Case 3.
```
given prices A, B and C
......Price A...|....Price B....|.......Price C.......>
adding price D
                            |...Price D...|
......Price A...|....Price B....|.......Price C.......>
result
......Price A...|..Price B..|...Price D...|..Price C..>
```
Having a product with several prices spread over time: A, B and C:
`A - startDate: 2020-03-01, endDate: 2020-05-31`
`B - startDate: 2020-06-01, endDate: 2020-08-31`   
`C - startDate: 2020-09-01, endDate: infinity`
Creating the price D, which overlaps partially price B and price C:
`D - startDate: 2020-07-01, endDate: 2020-10-31`   
as a result we will get:
`A - startDate: 2020-03-01, endDate: 2020-05-31`
`B - startDate: 2020-06-01, endDate: 2020-06-30`
`N - startDate: 2020-07-01, endDate: 2020-10-31`  
`D - startDate: 2020-11-01, endDate: infinity`

Case 4.
```
given prices A, B, C and D
......Price A...|..Price B..|..Price C..|..Price D...>
add price E
                          |....Price E....|
.....Price A....|..Price B..|..Price C..|..Price D...>
result
.....Price A....|.Price B.|....Price E....|.Price D..>
```
Having a product with prices A, B, C and D:
`A - startDate: 2020-03-01, endDate: 2020-05-31`
`B - startDate: 2020-06-01, endDate: 2020-08-31`
`C - startDate: 2020-09-01, endDate: 2020-11-30`
`D - startDate: 2020-12-01, endDate: infinity`
Creating a new price E, which overlaps partially price B, entire price C and partially price D:
`E - startDate: 2020-08-01, endDate: 2020-12-31`
as a result we will have:
`A - startDate: 2020-03-01, endDate: 2020-05-31`
`B - startDate: 2020-06-01, endDate: 2020-07-31`
`C - archived`
`N - startDate: 2020-08-01, endDate: 2020-12-31`   
`D - startDate: 2021-01-01, endDate: infinity`

## Deleting price
Keep in mind that only future prices can be deleted. It is not allowed to delete ongoing prices.
If the price to be deleted has overlapped other price when it was created, the previous state will be restored, as we keep the history of each price's lifetime.

Case 1.
```
given price A
....................Price A......................>
adding price B
........Price A........|........Price B..........>
deleting price B
expect
....................Price A.....................>
```
Having a product with two prices A and B, where B was created on top of price A:
`A - startDate: 2020-03-01, endDate: 2020-09-30`
`B - startDate: 2020-10-01, endDate: infinity`
after deleting B, as a result we will get price A restored:   
`A - startDate: 2020-03-01, endDate: infinity`


Case 2. 
```
given
......A.....|...X...|.......B.........>
expect
..........A.........|.......B.........>
```
we have product with three prices A, X and B: 
`A - startDate: current, endDate: 2020-08-31` 
`X - startDate: 2020-09-01, endDate: 2020-11-30` 
`B - startDate: 2020-12-01, endDate: infinity` 
after deleting X, as a result we will get:   
`A - startDate: current, endDate: 2020-11-30` 
`B - startDate: 2020-12-01, endDate: infinity`

Case 3.
```
given
......Price A......|...........Price B.............>
......Price A......|...Price C...|.....Price B.....>
expect
......Price A......|...........Price B.............>
```
in this case at the beginning there were two prices A and B:
`A - startDate: 2020-03-01, endDate: 2020-10-31`
`B - startDate: 2020-11-01, endDate: infinity`
then price C was created, which overlapped only B, which resulted in having following prices:
`A - startDate: 2020-03-01, endDate: 2020-10-31`
`C - startDate: 2020-11-01, endDate: 2020-11-30`
`B - startDate: 2020-12-01, endDate: infinity`
after deleting price C, as a result we get back the previous timeline

Case 4.
```
given (NP - no price in this period)
......Price A......|...NP...|......Price B.........>
expect
......Price A......|..............NP...............>
```
In this case between price A and B is a gap without price:
`A - startDate: 2020-03-01, endDate: 2020-08-31`
`B - startDate: 2020-11-01, endDate: infinity`
Deleting price C will result in not having price from end of price A to infinity

Case 5.
```
given -> price X which has previously overlapped 5 other prices
......Price A....|...............Price X..................|..Price E...>

                 |...............Price X..................|
......Price A.......|..Price B..|..Price C..|..Price D..|....Price E....>
expect
......Price A.......|..Price B..|..Price C..|..Price D..|....Price E....>
```
First diagram shows a current state: three prices A, X and E, but last created price X has overlapped the previous prices A, B, C, D, and E (see second diagram) when it was created. Current state of all prices:
`A - startDate: 2020-03-01, endDate: 2020-08-31`
`B - archived`
`C - archived`
`D - archived`
`E - startDate: 2021-04-01, endDate: infinity`
`X - startDate: 2020-09-01, endDate: 2021-03-31`
If price X will be deleted, service will restore the previously overlapped prices (see last diagram):
`A - startDate: 2020-03-01, endDate: 2020-09-30`
`B - startDate: 2020-10-01, endDate: 2020-11-30`
`C - startDate: 2020-12-01, endDate: 2021-01-31`
`D - startDate: 2021-02-01, endDate: 2021-03-31`
`E - startDate: 2021-04-01, endDate: infinity`

Price restoring feature is limited only to a scope of price that is being deleted and historical data of changes that were done because of the creation of that price. If any price that would be restored was deleted meanwhile, then it will not be restored and there will be a gap in price schedule in this period.

