# Price API guide
The Price API allows you to manage product prices for your store. It offers two methods for price management:

### 1. Through Product Creation/Update (Product API):

- Prices can be defined within the product creation or update process using the `prices` node in the Product API request body.
- Prices created this way are automatically synced with the Price API, with a validity period from product creation to infinity.

**Example**
> PUT `/products` - request body:

```json
{
  ...
  "prices": {
     "defaultCurrency": "USD",
     "priceByCountryByCurrency": {
        "USD": {
           "default": {
              "value": 100,
              "vatIncluded": true // default true
           }
        },
        "EUR": {
           "default": { "value": 200 },
           "FR": { "value": 300 },
           "DE": { "value": 400 }
        }
     }
  }
}
```

### 2. Directly Through the Price API:
- This method provides more granular control over prices.
- You can create prices, schedule them for the future, update existing prices (if not yet active), and delete scheduled prices.

**Example**

> POST `/prices` - request body: 

```json
{
  "customerId": "myCustomerId",
  "productId": "myProductId",
  "startDate": 1569669720000,
  "endDate": 1574943720000, // if not declared, endDate will be infinity
  "country": "FR",
  "currency": "EUR",
  "msrp": 200.00,
  "value": 130.00,
  "vatIncluded": false
}
```

## Price Object Structure

When creating prices directly through the Price API, the following fields are required:

- `customerId`: Unique identifier of the customer (e.g., "60f70f89-0498-487e-ba55-2cac045d4171")
- `productId`: Unique identifier of the product (e.g., "60f70f89-0498-487e-ba55-2cac045d4171")
- `startDate`: Price start date in milliseconds since epoch (e.g., 1589439239780)
- `currency`: Currency code in ISO-4217 format (e.g., EUR)
- `value`: Regular price with two decimal places (e.g., 14.99)
- `vatIncluded`: Boolean flag indicating if tax is included in the price (true or false)

### Additional Price Object Fields (Optional):

- `endDate`: Price end date in milliseconds since - epoch (if not specified, endDate will be infinity)
- `country`: Country code in ISO 3166-2 format (e.g., FR) - restricts price to a specific country
- `msrp`: Manufacturer suggested retail price with two decimal places (e.g., 29.99)
- `marketingCampaignId`: Marketing campaign ID - restricts price only to a specific marketing campaign
- `archived`: Indicator if the price was removed/archived (read-only)


## getBestPrice endpoint

This endpoint retrieves the most suitable price for a given product based on specified parameters. It's helpful for determining the price displayed in a shopping cart.

> GET /prices/best

### Required Parameters:

- `customerId`
- `productId`
- `country`

### Optional Parameters:

- `currency`
- `defaultCurrency` (fallback currency if no price found for the requested currency)
- `marketingCampaignId`
- `date` (defaults to the current date if not specified)

### Price Search Logic:

The search considers only prices valid on the specified date (or current date if not provided).
Gaps between price validities indicate invalid product configuration.
Marketing campaign prices take priority over regular prices.
If no price is found in the requested currency, the defaultCurrency is used (if provided).


Let's find out how the search works in details.

1. Date

   - If no `date` is provided, the search considers prices valid on the current date.
   -  Gaps between price validity periods (e.g., a price ending on October 31st, 2020, and another starting on January 1st, 2021) indicate an invalid product configuration.
 
      **Example**: Consider a product with two prices A and B defined as follows:
      ```
      ......A......|...NoPrice...|.....B......>
      A - startDate: 2020-01-01, endDate: 2020-10-31
      B - startDate: 2021-01-01, endDate: none
      ```
      
      Price requests for dates within the gap (November & December 2020) will result in "no price found" because prices A and B are not valid during that period.


2. Marketing Campaign Id

   - If a `marketingCampaignId` is provided, prices associated with that campaign take priority over regular prices (without a campaign ID).
   - The search will first look for campaign-specific prices and then fall back to regular prices if none are found.

3. Currency

   - If a specific `currency` is requested, the search returns prices only in that currency.
   - The service will never convert prices to a different currency.

4. Country
   - This is a required parameter.
   - The search prioritizes prices defined for the specific country.
   - If no country-specific prices are found, the search falls back to the default currency for that country.
   - **Example**: Consider a product with prices defined for different countries:
      ```json
      "prices":{
         "defaultCurrency":"EUR",
         "priceByCountryByCurrency":{
            "EUR":{
               "default":{ "value":2000 },
               "FR":{ "value":1899 },
               "DE":{ "value":899 }
            }
         }
      }
      ```
      - Searching for a price with `currency=EUR` and `country=FR` will return 1899 EUR.
      - Searching for a price with `currency=EUR` and `country=DE` will return 899 EUR.
      - Searching for a price with `currency=EUR` and `country=ES` (no price for ES defined) will return the default price (2000 EUR).

5. Default Currency
      - If no prices are found based on the previous criteria and a `defaultCurrency` parameter is provided, the search attempts to find a price in that fallback currency.
      - **Example**: With the product example above, searching for a price with `country=US` and `defaultCurrency=EUR` will return the default EUR price (2000 EUR).

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

If the service can't find any price, it returns an error:

> Price not found for product "productId" for "given country" on date "given date"

```json
{
   "timestamp": 1594109151537,
   "status": 404,
   "error": "Not Found",
   "message": "Price not found for product 7b042106-614c-4d1e-9247-c9f394215368 for CH on date 12 Oct 2030 00:00:00 GMT",
   "path": "/prices/best"
}
```

## Price overlapping

When creating a new price with a validity period that overlaps existing prices, the Price API automatically adjusts price validity periods of existing prices to prevent conflicts. Here's a breakdown of how it handles different overlap scenarios:

### Case 1: Overlap at the End of an Existing Price 
```
....................Price A......................>
                       +
                       |........Price B..........>
                       ⇓
........Price A........|........Price B..........>
```
1. **Given**: Price A exists with `startDate: 2020-03-01` and `endDate: infinity`.
2. **Adding**: Price B with `startDate: 2020-10-01`, `endDate: infinity`.
3. **Result**: Price A's endDate is adjusted to `2020-09-30` to accommodate Price B.

### Case 2: Overlap in the Middle of an Existing Price

```
.......................Price A........................>
                          +
                 |....Price B....|
                          ⇓
.....Price A.....|....Price B....|.....Price C........>
```

1. **Given**: Price A exists with `startDate: 2020-03-01` and `endDate: infinity`.
2. **Adding**: Price B with `startDate: 2020-10-01`, and `endDate: 2021-01-31` (partially overlaps A).
3. **Result**:
   - Price A's endDate is adjusted to `2020-09-30`.
   - Price B's `startDate: 2020-10-01`, `endDate: 2021-01-31`
   - A new price (C) is automatically created by the Price API with `startDate: 2021-02-01` and `endDate: infinity`.


### Case 3: Overlap with Multiple Existing Prices

```
......Price A...|....Price B....|.......Price C.......>
                        +
                          |...Price D.................>
                        ⇓
......Price A...|..Price B..|...Price D...............>
```

1. **Given**: Prices A (2020-03-01 to 2020-05-31), B (2020-06-01 to 2020-08-31), and C (2020-09-01 to infinity) exist.
2. **Adding**: Price D with `startDate: 2020-07-01` and `endDate: infinity` (overlaps B and C).
3. **Result**:
   - Price A remains unchanged.
   - Price B's endDate is adjusted to `2020-06-30`.
   - Price C is archived because it's entirely replaced by D.
   - Price D takes effect from `2020-07-01` to `infinity`.

### Key Points:

- The Price API prioritizes non-overlapping price definitions.
- Overlapping new prices are adjusted to fit alongside existing validities.
- In some cases (complete overlap), existing prices might be archived.
- New prices might be created to maintain price continuity.


## Deleting prices

The Price API allows you to delete prices. Prices with the validity periods in the future are deleted completely. Prices that are currently active (startDate in the past and endDate in the future) are archived. 

Here's what happens when you delete a price which has not started yet:

```
....................Price A......................>
adding price B:
........Price A........|........Price B..........>
deleting price B:
........Price A........|_________no price________>
```

1. **Given**: Price A: `startDate: 2020-03-01`, `endDate: infinity`
2. **Adding**: Price B: `startDate: 2020-10-01`, `endDate: infinity`
3. **Result**:
   - A: `startDate: 2020-03-01`, `endDate: 2020-09-30`
   - B: `startDate: 2020-10-01`, `endDate: infinity`
4. **After Deleting B**: Price A endDate doesn't change
   - A: `startDate: 2020-03-01`, `endDate: 2020-09-30`
