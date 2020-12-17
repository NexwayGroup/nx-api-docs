
## SearchResult«Store» resource
### Methods

- [get](/docs/nx-api-docs/docs/guides/manage%20stores/04-get-all-stores.md)
 - Get list of stores

### Resource

```json
{
    "items": [
        {
            "allowOrderDetailsOnCheckoutConfirmation": "boolean",
            "bannerInvoice": "string",
            "bannerOrderConfEmail": "string",
            "baseProduct": "string",
            "blackListedCountries": [
                "string"
            ],
            "blackListedPaymentMethods": [
                "string"
            ],
            "createDate": "date-time",
            "createEndUserWithoutSubscription": "boolean",
            "customerId": "string",
            "dbVersion": "int64",
            "defaultLocale": "string",
            "designs": {
                "checkout": {
                    "i18n": "string",
                    "layout": "string",
                    "theme": "string"
                },
                "endUserPortal": {
                    "i18n": "string",
                    "layout": "string",
                    "theme": "string"
                }
            },
            "emailSenderOverride": "string",
            "fallbackCartCountry": "string",
            "forceGeoipLocalization": "boolean",
            "gtmId": "string",
            "hostnames": [
                "string"
            ],
            "id": "string",
            "logoFavicon": "string",
            "logoStore": "string",
            "name": "string",
            "promoteOneClickPayment": "boolean",
            "saleLocales": [
                "string"
            ],
            "status": "string",
            "storeWebsite": "string",
            "targetNonRecurrentPaymentMethodsForSubscriptions": "string",
            "thankYouDesc": {
                "<key>": "string"
            },
            "theme": "string",
            "updateDate": "date-time"
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

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|items[] 	|array| 		|Optional| 
|items[].allowOrderDetailsOnCheckoutConfirmation| 	boolean|Allow order details on checkout confirmation|Optional|
|items[].bannerInvoice| 	string|Invoice banner|	Optional|
|items[].bannerOrderConfEmail| 	string|Order confirmation banner|	Optional|
|items[].baseProduct| 	string|Product ID of store|	Optional|
|items[].blackListedCountries[]| 	array of string 	|List of countries not authorized for this store|Optional|
|items[].blackListedPaymentMethods[]| 	array of string 	|List of payment methods not authorized for this store|	Optional|
|items[].createDate| 	date-time|| 		Optional|
|items[].createEndUserWithoutSubscription| 	boolean|Create an end-user whatever the product type bought (Permanent / Subscription)|Optional|
|items[].customerId| 	string|Customer ID of store|	Optional|
|items[].dbVersion| 	int64|| 		Optional|
|items[].defaultLocale| 	string|Store default locale||	
|items[].designs| 	object| 	Designs| 	Optional|
|items[].designs.checkout| 	object| 	Design| 	Optional|
|items[].designs.checkout.i18n |	string|Name of the set of i18n files to use in this context|Optional|
|items[].designs.checkout.layout| 	string|Name of the layout to use in this context|	Optional|
|items[].designs.checkout.theme| 	string|Name of the theme to use in this context|Optional|
|items[].designs.endUserPortal 	|object| 	Design| 	Optional|
|items[].designs.endUserPortal.i18n| 	string|Name of the set of i18n files to use in this context|	Optional|
|items[].designs.endUserPortal.layout 	|string|Name of the layout to use in this context|Optional|
|items[].designs.endUserPortal.theme| 	string|Name of the theme to use in this context|Optional|
|items[].emailSenderOverride| 	string|Value to used to build email sender; for example, if set to xxx, the sender will be: noreply.xxx@nexway\.com|Optional|
|items[].fallbackCartCountry| 	string|Fallback cart country|	Optional|
|items[].forceGeoipLocalization| 	boolean|Limit end-user country to GeoIP location|Optional|
|items[].gtmId| 	string|Google Tag Manager ID|	Optional|
|items[].hostnames[]| 	array of string|List of hostnames for accessing cart and end-user portal||
|items[].id| 	string| 		|Optional|
|items[].logoFavicon| 	string|Favicon logo|	Optional|
|items[].logoStore| 	string|Store logo|	Optional|
|items[].name| 	string|Store name||
|items[].promoteOneClickPayment| 	boolean|Promote one-click payment|	Optional|
|items[].saleLocales[]| 	array of string|Locale where store is available||
|items[].status| 	string|Store status. Possible values are: `ENABLED`,`DISABLED`|	Optional|
|items[].storeWebsite| 	string|Store website|	Optional|
|items[].targetNonRecurrentPaymentMethodsForSubscriptions| 	string|Target non-recurrent payment methods for subscriptions. Possible values are:`EVERYBODY`,`NOBODY`,`PROFESSIONAL`,`COMPANY`|	Optional|
|items[].thankYouDesc| 	object|Thank you description|Optional|
|items[].thankYouDesc.<key\>| 	map of string|| 		Optional
|items[].theme| 	string|Deprecated|	Optional, read only. |
|items[].updateDate |	date-time ||		Optional |
|last| 	boolean|Last page or not|	Read only.|
|number| 	int32|Current page number|	Read only.|
|size|	int32|Number of carts per page|	Read only.|
|totalItems| 	int64|Total number of carts|	Read only.|
|totalPages| 	int32|Total number of pages|	Read only.|
