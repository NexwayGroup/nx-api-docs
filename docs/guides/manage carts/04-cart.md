
## Cart resource
### Methods

- [get](/docs/guides/manage%20carts/01-get-cart.md) - Get a cart by id
- [get](https://api-doc.nexway.store/nexway-monetize/reference/shopper-apis/get-public-cart?v=latest) - Get a cart by id
- [put](https://api-doc.nexway.store/nexway-monetize/reference/shopper-apis/update-cart?v=latest) - Update a cart
- [put](https://api-doc.nexway.store/nexway-monetize/reference/shopper-apis/update-products-in-cart?v=latest) - Update products to a cart

### Resource

```json
{
    "checkoutUrl": "string",
    "country": "string",
    "createDate": "date-time",
    "currency": "string",
    "customerId": "string",
    "dbVersion": "int64",
    "discounts": [
        "string"
    ],
    "discountsStatus": [
        {
            "discount": "string",
            "discountLabel": "string",
            "status": "string"
        }
    ],
    "endUser": {
        "city": "string",
        "company": {
            "companyName": "string",
            "validVatNumber": "boolean",
            "vatNumber": "string"
        },
        "country": "string",
        "email": "string",
        "firstName": "string",
        "id": "string",
        "lastName": "string",
        "locale": "string",
        "streetAddress": "string",
        "zipCode": "string"
    },
    "externalContext": "string",
    "id": "string",
    "locale": "string",
    "modelVersion": "string",
    "notReadyForOrderCauseMessage": "string",
    "price": {
        "currency": "string",
        "discountedPrice": {
            "discountRate": "double",
            "discountedGrossPrice": "double",
            "discountedNetPrice": "double",
            "grossPriceDiscountAmount": "double",
            "netPriceDiscountAmount": "double",
            "vatDiscountAmount": "double"
        },
        "grossPrice": "double",
        "netPrice": "double",
        "vatAmount": "double",
        "vatRate": "double"
    },
    "products": [
        {
            "codeName": "string",
            "customerId": "string",
            "externalContext": "string",
            "fulfillmentTemplate": "string",
            "fullPrice": {
                "currency": "string",
                "discountedPrice": {
                    "discountRate": "double",
                    "discountedGrossPrice": "double",
                    "discountedNetPrice": "double",
                    "grossPriceDiscountAmount": "double",
                    "netPriceDiscountAmount": "double",
                    "vatDiscountAmount": "double"
                },
                "grossPrice": "double",
                "netPrice": "double",
                "vatAmount": "double",
                "vatRate": "double"
            },
            "id": "string",
            "lifeTime": "string",
            "name": "string",
            "parentId": "string",
            "priceFunctionParameters": [
                {
                    "defaultValue": {},
                    "field": "string",
                    "internalDesc": "string",
                    "label": "string",
                    "max": "int32",
                    "min": "int32",
                    "pattern": "string",
                    "type": "string"
                }
            ],
            "priceFunctionParametersValues": {
                "<key>": "string"
            },
            "publisherRefId": "string",
            "quantity": "int32",
            "relatedContents": [
                {
                    "file": "string",
                    "label": "string"
                }
            ],
            "signedProduct": "string",
            "subscriptionTemplate": "string",
            "type": "string",
            "unitPrice": {
                "currency": "string",
                "discountedPrice": {
                    "discountRate": "double",
                    "discountedGrossPrice": "double",
                    "discountedNetPrice": "double",
                    "grossPriceDiscountAmount": "double",
                    "netPriceDiscountAmount": "double",
                    "vatDiscountAmount": "double"
                },
                "grossPrice": "double",
                "netPrice": "double",
                "vatAmount": "double",
                "vatRate": "double"
            },
            "variableValues": {}
        }
    ],
    "salesFlag": [
        "string"
    ],
    "status": "string",
    "storeHostname": "string",
    "storeId": "string",
    "totalAmount": "double",
    "updateDate": "date-time",
    "version": "int64"
}
```

### Properties

| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|checkoutUrl| 	string |The full URL used by cart|Optional|
|country| 	string 	|Country of the cart||
|createDate| 	date-time|| 		Optional|
|currency| 	string|Currency of the cart||
|customerId| 	string|Customer linked to the cart||
|dbVersion| 	int64| 		|Optional|
|discounts[]| 	array of string|List of signed discounts|	Optional|
|discountsStatus[]| 	array 	|List of discounts status|	Optional|
|discountsStatus[].discount| 	string|| 		Optional|
|discountsStatus[].discountLabel| 	string|| 		Optional|
|discountsStatus[].status| 	string|| 		Optional|
|endUser| 	object| 	EndUser||
|endUser.city| 	string|Address book city|	Optional|
|endUser.company| 	object| 	Company| 	Optional|
|endUser.company.companyName| 	string|Enduser company name||
|endUser.company.validVatNumber| 	boolean| 		|Optional|
|endUser.company.vatNumber| 	string|Company vat number|	Optional|
|endUser.country| 	string|This is the country end user.|	Optional|
|endUser.email| 	string|This is the email end user.||	
|endUser.firstName| 	string|End user first name|Optional|
|endUser.id| 	string|This is the reference id of the end user service.|	Optional|
|endUser.lastName| 	string|End user last name|	Optional|
|endUser.locale| 	string|End user locale||
|endUser.streetAddress| 	string|Address book street address||
|endUser.zipCode| 	string|Address book zipCode||
|externalContext| 	string|External context|Optional|
|id| 	string| 		|Optional|
|locale| 	string|Locale of the cart|	Optional, read only.|
|modelVersion| 	string|| 		Optional|
|notReadyForOrderCauseMessage| 	string| 		|Optional|
|price| 	object| 	FullPrice|| 	
|price.currency| 	string|Currency (ISO-4217) of the amount||
|price.discountedPrice| 	object| 	DiscountedPrice| 	Optional|
|price.discountedPrice.discountRate| 	double|Discount rate||	
|price.discountedPrice.discountedGrossPrice| 	double|This is the discounted amount of the product with taxes included||
|price.discountedPrice.discountedNetPrice| 	double|This is the discounted amount of the product without taxes included||
|price.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||
|price.discountedPrice.netPriceDiscountAmount| 	double|Net price discount amount||
|price.discountedPrice.vatDiscountAmount| 	double|VAT discount amount||
|price.grossPrice| 	double|This is the amount of the product with taxes included.|Optional|
|price.netPrice| 	double|Net price||
|price.vatAmount| 	double|this is the vat amount (amount - netAmount)||	
|price.vatRate| 	double|Sales tax rate||
|products[]| 	array|List of products in the cart||	
|products[].codeName| 	string|Customer internal product name|	Optional, read only.|
|products[].customerId| 	string|Customer id of the product|	Read only.|
|products[].externalContext| 	string|External context|	Optional|
|products[].fulfillmentTemplate| 	string|Fulfillment template name|	Optional|
|products[].fullPrice| 	object| 	FullPrice||	
|products[].fullPrice.currency| 	string|Currency (ISO-4217) of the amount||
|products[].fullPrice.discountedPrice 	|object| 	DiscountedPrice| 	Optional|
|products[].fullPrice.discountedPrice.discountRate| 	double|Discount rate||
|products[].fullPrice.discountedPrice.discountedGrossPrice| 	double|This is the discounted amount of the product with taxes included||
|products[].fullPrice.discountedPrice.discountedNetPrice 	|double|This is the discounted amount of the product without taxes included||
|products[].fullPrice.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||	
|products[].fullPrice.discountedPrice.netPriceDiscountAmount 	|double|Net price discount amount||	
|products[].fullPrice.discountedPrice.vatDiscountAmount 	|double|VAT discount amount||
|products[].fullPrice.grossPrice| 	double|This is the amount of the product with taxes included.|	Optional|
|products[].fullPrice.netPrice| 	double|Net price||	
|products[].fullPrice.vatAmount| 	double|this is the vat amount (amount - netAmount)||
|products[].fullPrice.vatRate| 	double|Sales tax rate||
|products[].id| 	string|Id of the product|	Read only.|
|products[].lifeTime| 	string|lifeTime||	
|products[].name 	|string|Product name|	Optional|
|products[].parentId| 	string|Parent id of the product|	Optional, read only.|
|products[].priceFunctionParameters[] 	|array|Parameters of the product||	
|products[].priceFunctionParameters[].defaultValue| 	object|Name of the parameter||
|products[].priceFunctionParameters[].field| 	string|Name of the parameter||
|products[].priceFunctionParameters[].internalDesc| 	string|Internal description of the parameter|	Optional|
|products[].priceFunctionParameters[].label 	|string|The label of the parameter||	
|products[].priceFunctionParameters[].max| 	int32|Max value||	
|products[].priceFunctionParameters[].min| 	int32|Min value||	
|products[].priceFunctionParameters[].pattern| 	string|Pattern value||	
|products[].priceFunctionParameters[].type| 	string|The type of the parameter||	
|products[].priceFunctionParametersValues| 	object|Values parameters of the product||	
|products[].priceFunctionParametersValues.<key\>| 	map of string|| 		Optional|
|products[].publisherRefId| 	string|Publisher reference of the product||
|products[].quantity| 	int32|Product quantity.||	
|products[].relatedContents[]| 	array|Links for download the related contents file of product|Optional|
|products[].relatedContents[].file| 	string|URL or file name with extension for download|	Optional|
|products[].relatedContents[].label| 	string|Text for explain what is this binary|	Optional|
|products[].signedProduct| 	string|Signed product|	Optional|
|products[].subscriptionTemplate| 	string|Subscription template name|	Optional|
|products[].type| 	string|Type of product. Possible values are:`SOFTWARE`,`GAMES`,`CASUAL`,`SERVICE`|	Optional|
|products[].unitPrice| 	object| 	FullPrice||	
|products[].unitPrice.currency| 	string|Currency (ISO-4217) of the amount||	
|products[].unitPrice.discountedPrice| 	object| 	DiscountedPrice| 	Optional|
|products[].unitPrice.discountedPrice.discountRate| 	double|Discount rate||	
|products[].unitPrice.discountedPrice.discountedGrossPrice 	|double|This is the discounted amount of the product with taxes included||	
|products[].unitPrice.discountedPrice.discountedNetPrice 	|double|This is the discounted amount of the product without taxes included||	
|products[].unitPrice.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||	
|products[].unitPrice.discountedPrice.netPriceDiscountAmount| 	double|Net price discount amount||	
|products[].unitPrice.discountedPrice.vatDiscountAmount| 	double|VAT discount amount||
|products[].unitPrice.grossPrice| 	double|This is the amount of the product with taxes included.|	Optional|
|products[].unitPrice.netPrice| 	double|Net price||	
|products[].unitPrice.vatAmount| 	double|this is the vat amount (amount - netAmount)||
|products[].unitPrice.vatRate| 	double|Sales tax rate||
|products[].variableValues| 	object|Variables of the product|	Optional|
|salesFlag[] 	|array of string|List of sales flags|	Optional|
|status| 	string|Cart status. Possible values are:`CREATED`,`VALIDATED`,`FAILED`,`CANCEL`,`DELETED`|	Read only.|
|storeHostname| 	string|The hostname of store|	Optional|
|storeId| 	string|Store linked to the cart||	
|totalAmount| 	double|The total amount of this order||	
|updateDate| 	date-time|| 		Optional|
|version| 	int64|| 		Optional| 

### Example

```json
{
    "country": "US",
    "currency": "USD",
    "endUser": {
        "city": "San Francisco",
        "country": "US",
        "email": "jdoe@com2us.com",
        "firstName": "Doe",
        "lastName": "John",
        "locale": "en-US",
        "streetAddress": "236 8th street",
        "id": "82fc0517-358a-49a8-b5fb-8be1a0ced674",
        "storeId": "a0253398-b0c5-4c2d-a15f-bd97ea4be608",
        "zipCode": "94103"
    },
    "locale": "en-US",
    "products": {
        "customerId": "a0253398-b0c5-4c2d-a15f-bd97ea4be60",
        "fulfillmentTemplate": "com2us",
        "id": "f5863398-545e-ac2d-4c5f-db58a2ebe60",
        "lifeTime": "2YEARS",
        "name": "productName",
        "parentId": "edf53387-85e5-4d8a-b574-adb5842be67",
        "price": {
            "currency": "USD",
            "discountedPrice": {
                "discountRate": 0.5,
                "discountedGrossPrice": 71.4,
                "discountedNetPrice": 59.5,
                "grossPriceDiscountAmount": 71.4,
                "netPriceDiscountAmount": 59.5,
                "vatDiscountAmount": 11.9
            },
            "grossPrice": 142.8,
            "netPrice": 119,
            "vatAmount": 23.8,
            "vatRate": 0.2
        },
        "publisherRefId": "SKU0001",
        "quantity": 2,
        "subscriptionTemplate": "CUSTOMER_ABO_1Y",
        "type": "SOFTWARE"
    },
    "status": "CREATED",
    "storeId": "a0253398-b0c5-4c2d-a15f-bd97ea4be608",
    "totalAmount": 285.6,
    "updateDate": "1479225750482",
    "version": 3
}
```
