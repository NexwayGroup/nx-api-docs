
## CartSearchResult resource
### Methods

[get](/docs/nx-api-docs/docs/guides/manage%20carts/02-get-all-carts.md) - Get list of carts

### Resource

```json
{
    "items": [
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
    ],
    "last": "boolean",
    "number": "int32",
    "size": "int32",
    "totalItems": "int64",
    "totalPages": "int32"
}
```

### Properties

| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|items[]| 	array|List of carts||
|items[].checkoutUrl| 	string |The full URL used by cart|Optional|
|items[].country| 	string 	|Country of the cart||
|items[].createDate| 	date-time|| 		Optional|
|items[].currency| 	string|Currency of the cart||
|items[].customerId| 	string|Customer linked to the cart||
|items[].dbVersion| 	int64| 		|Optional|
|items[].discounts[]| 	array of string|List of signed discounts|	Optional|
|items[].discountsStatus[]| 	array 	|List of discounts status|	Optional|
|items[].discountsStatus[].discount| 	string|| 		Optional|
|items[].discountsStatus[].discountLabel| 	string|| 		Optional|
|items[].discountsStatus[].status| 	string|| 		Optional|
|items[].endUser| 	object| 	EndUser||
|items[].endUser.city| 	string|Address book city|	Optional|
|items[].endUser.company| 	object| 	Company| 	Optional|
|items[].endUser.company.companyName| 	string|Enduser company name||
|items[].endUser.company.validVatNumber| 	boolean| 		|Optional|
|items[].endUser.company.vatNumber| 	string|Company vat number|	Optional|
|items[].endUser.country| 	string|This is the country end user.|	Optional|
|items[].endUser.email| 	string|This is the email end user.||	
|items[].endUser.firstName| 	string|End user first name|Optional|
|items[].endUser.id| 	string|This is the reference id of the end user service.|	Optional|
|items[].endUser.lastName| 	string|End user last name|	Optional|
|items[].endUser.locale| 	string|End user locale||
|items[].endUser.streetAddress| 	string|Address book street address||
|items[].endUser.zipCode| 	string|Address book zipCode||
|items[].externalContext| 	string|External context|Optional|
|items[].id| 	string| 		|Optional|
|items[].locale| 	string|Locale of the cart|	Optional, read only.|
|items[].modelVersion| 	string|| 		Optional|
|items[].notReadyForOrderCauseMessage| 	string| 		|Optional|
|items[].price| 	object| 	FullPrice|| 	
|items[].price.currency| 	string|Currency (ISO-4217) of the amount||
|items[].price.discountedPrice| 	object| 	DiscountedPrice| 	Optional|
|items[].price.discountedPrice.discountRate| 	double|Discount rate||	
|items[].price.discountedPrice.discountedGrossPrice| 	double|This is the discounted amount of the product with taxes included||
|items[].price.discountedPrice.discountedNetPrice| 	double|This is the discounted amount of the product without taxes included||
|items[].price.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||
|items[].price.discountedPrice.netPriceDiscountAmount| 	double|Net price discount amount||
|items[].price.discountedPrice.vatDiscountAmount| 	double|VAT discount amount||
|items[].price.grossPrice| 	double|This is the amount of the product with taxes included.|Optional|
|items[].price.netPrice| 	double|Net price||
|items[].price.vatAmount| 	double|this is the vat amount (amount - netAmount)||	
|items[].price.vatRate| 	double|Sales tax rate||
|items[].products[]| 	array|List of products in the cart||	
|items[].products[].codeName| 	string|Customer internal product name|	Optional, read only.|
|items[].products[].customerId| 	string|Customer id of the product|	Read only.|
|items[].products[].externalContext| 	string|External context|	Optional|
|items[].products[].fulfillmentTemplate| 	string|Fulfillment template name|	Optional|
|items[].products[].fullPrice| 	object| 	FullPrice||	
|items[].products[].fullPrice.currency| 	string|Currency (ISO-4217) of the amount||
|items[].products[].fullPrice.discountedPrice 	|object| 	DiscountedPrice| 	Optional|
|items[].products[].fullPrice.discountedPrice.discountRate| 	double|Discount rate||
|items[].products[].fullPrice.discountedPrice.discountedGrossPrice| 	double|This is the discounted amount of the product with taxes included||
|items[].products[].fullPrice.discountedPrice.discountedNetPrice 	|double|This is the discounted amount of the product without taxes included||
|items[].products[].fullPrice.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||	
|items[].products[].fullPrice.discountedPrice.netPriceDiscountAmount 	|double|Net price discount amount||	
|items[].products[].fullPrice.discountedPrice.vatDiscountAmount 	|double|VAT discount amount||
|items[].products[].fullPrice.grossPrice| 	double|This is the amount of the product with taxes included.|	Optional|
|items[].products[].fullPrice.netPrice| 	double|Net price||	
|items[].products[].fullPrice.vatAmount| 	double|this is the vat amount (amount - netAmount)||
|items[].products[].fullPrice.vatRate| 	double|Sales tax rate||
|items[].products[].id| 	string|Id of the product|	Read only.|
|items[].products[].lifeTime| 	string|lifeTime||	
|items[].products[].name 	|string|Product name|	Optional|
|items[].products[].parentId| 	string|Parent id of the product|	Optional, read only.|
|items[].products[].priceFunctionParameters[] 	|array|Parameters of the product||	
|items[].products[].priceFunctionParameters[].defaultValue| 	object|Name of the parameter||
|items[].products[].priceFunctionParameters[].field| 	string|Name of the parameter||
|items[].products[].priceFunctionParameters[].internalDesc| 	string|Internal description of the parameter|	Optional|
|items[].products[].priceFunctionParameters[].label 	|string|The label of the parameter||	
|items[].products[].priceFunctionParameters[].max| 	int32|Max value||	
|items[].products[].priceFunctionParameters[].min| 	int32|Min value||	
|items[].products[].priceFunctionParameters[].pattern| 	string|Pattern value||	
|items[].products[].priceFunctionParameters[].type| 	string|The type of the parameter||	
|items[].products[].priceFunctionParametersValues| 	object|Values parameters of the product||	
|items[].products[].priceFunctionParametersValues.<key\>| 	map of string|| 		Optional|
|items[].products[].publisherRefId| 	string|Publisher reference of the product||
|items[].products[].quantity| 	int32|Product quantity.||	
|items[].products[].relatedContents[]| 	array|Links for download the related contents file of product|Optional|
|items[].products[].relatedContents[].file| 	string|URL or file name with extension for download|	Optional|
|items[].products[].relatedContents[].label| 	string|Text for explain what is this binary|	Optional|
|items[].products[].signedProduct| 	string|Signed product|	Optional|
|items[].products[].subscriptionTemplate| 	string|Subscription template name|	Optional|
|items[].products[].type| 	string|Type of product. Possible values are:`SOFTWARE`,`GAMES`,`CASUAL`,`SERVICE`|	Optional|
|items[].products[].unitPrice| 	object| 	FullPrice||	
|items[].products[].unitPrice.currency| 	string|Currency (ISO-4217) of the amount||	
|items[].products[].unitPrice.discountedPrice| 	object| 	DiscountedPrice| 	Optional|
|items[].products[].unitPrice.discountedPrice.discountRate| 	double|Discount rate||	
|items[].products[].unitPrice.discountedPrice.discountedGrossPrice 	|double|This is the discounted amount of the product with taxes included||	
|items[].products[].unitPrice.discountedPrice.discountedNetPrice 	|double|This is the discounted amount of the product without taxes included||	
|items[].products[].unitPrice.discountedPrice.grossPriceDiscountAmount 	|double|Gross price discount amount||	
|items[].products[].unitPrice.discountedPrice.netPriceDiscountAmount| 	double|Net price discount amount||	
|items[].products[].unitPrice.discountedPrice.vatDiscountAmount| 	double|VAT discount amount||
|items[].products[].unitPrice.grossPrice| 	double|This is the amount of the product with taxes included.|	Optional|
|items[].products[].unitPrice.netPrice| 	double|Net price||	
|items[].products[].unitPrice.vatAmount| 	double|this is the vat amount (amount - netAmount)||
|items[].products[].unitPrice.vatRate| 	double|Sales tax rate||
|items[].products[].variableValues| 	object|Variables of the product|	Optional|
|items[].salesFlag[] 	|array of string|List of sales flags|	Optional|
|items[].status| 	string|Cart status. Possible values are:`CREATED`,`VALIDATED`,`FAILED`,`CANCEL`,`DELETED`|	Read only.|
|items[].storeHostname| 	string|The hostname of store|	Optional|
|items[].storeId| 	string|Store linked to the cart||	
|items[].totalAmount| 	double|The total amount of this order||	
|items[].updateDate| 	date-time|| 		Optional|
|items[].version| 	int64|| 		Optional| 
|last| 	boolean|Last page or not|	Read only.|
|number| 	int32|Current page number|	Read only.|
|size|	int32|Number of carts per page|	Read only.|
|totalItems| 	int64|Total number of carts|	Read only.|
|totalPages| 	int32|Total number of pages|	Read only.|
