
## Create a store

Create a new store entry
### Request

`POST https://api.nexway.store/stores`

### Request body

The request body takes a complete [Store resource](/docs/guides/manage%20stores/07-store.md), containing the following writable properties:

```json
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
    "updateDate": "date-time"
}
```

## Properties

|Name| 	Type| 	Description| 	Additional|
|--|--|--|--|
|allowOrderDetailsOnCheckoutConfirmation| 	boolean|Allow order details on checkout confirmation|Optional|
|bannerInvoice| 	string|Invoice banner|	Optional|
|bannerOrderConfEmail| 	string|Order confirmation banner|	Optional|
|baseProduct| 	string|Product ID of store|	Optional|
|blackListedCountries[]| 	array of string 	|List of countries not authorized for this store|Optional|
|blackListedPaymentMethods[]| 	array of string 	|List of payment methods not authorized for this store|	Optional|
|CreateDate| 	date-time|| 		Optional|
|createEndUserWithoutSubscription| 	boolean|Create an end-user whatever the product type bought (Permanent / Subscription)|Optional|
|customerId| 	string|Customer ID of store|	Optional|
|dbVersion| 	int64|| 		Optional|
|defaultLocale| 	string|Store default locale||	
|designs| 	object| 	Designs| 	Optional|
|designs.checkout| 	object| 	Design| 	Optional|
|designs.checkout.i18n |	string|Name of the set of i18n files to use in this context|Optional|
|designs.checkout.layout| 	string|Name of the layout to use in this context|	Optional|
|designs.checkout.theme| 	string|Name of the theme to use in this context|Optional|
|designs.endUserPortal 	|object| 	Design| 	Optional|
|designs.endUserPortal.i18n| 	string|Name of the set of i18n files to use in this context|	Optional|
|designs.endUserPortal.layout 	|string|Name of the layout to use in this context|Optional|
|designs.endUserPortal.theme| 	string|Name of the theme to use in this context|Optional|
|emailSenderOverride| 	string|Value to used to build email sender; for example, if set to xxx, the sender will be: noreply.xxx@nexway\.com|Optional|
|fallbackCartCountry| 	string|Fallback cart country|	Optional|
|forceGeoipLocalization| 	boolean|Limit end-user country to GeoIP location|Optional|
|gtmId| 	string|Google Tag Manager ID|	Optional|
|hostnames[]| 	array of string|List of hostnames for accessing cart and end-user portal||
|id| 	string| 		|Optional|
|logoFavicon| 	string|Favicon logo|	Optional|
|logoStore| 	string|Store logo|	Optional|
|name| 	string|Store name||
|promoteOneClickPayment| 	boolean|Promote one-click payment|	Optional|
|saleLocales[]| 	array of string|Locale where store is available||
|status| 	string|Store status. Possible values are: `ENABLED`,`DISABLED`|	Optional|
|storeWebsite| 	string|Store website|	Optional|
|targetNonRecurrentPaymentMethodsForSubscriptions| 	string|Target non-recurrent payment methods for subscriptions. Possible values are:`EVERYBODY`,`NOBODY`,`PROFESSIONAL`,`COMPANY`|	Optional|
|thankYouDesc| 	object|Thank you description|Optional|
|thankYouDesc.<key\>| 	map of string|| 		Optional
|updateDate| 	date-time|| 		Optional|

### Authorisation

You need to have a valid JWT to access this API. Please read [JWT Authentication](/docs/guides/JWT%20Authentication/01-summary.md).
This request requires the use of one of following authorisation methods: `OAuth2`.
### Response

The following HTTP status codes may be returned, optionally with a response resource.

|Status code| 	Description|Resource|
|--|--|--|
|201| 	**Created** Success ||
|401| 	**Unauthorized** Unauthorized| |
|403| 	**Forbidden** Forbidden||
|404| 	**Not Found** Not Found||
|500| 	**Internal Server Error** Failure||

### Example

```json
{
    "bannerInvoice": "https://mywebsite.com/images/banner_invoice.png",
    "bannerOrderConfEmail": "https://mywebsite.com/images/banner_orderconfemail.png",
    "customerId": "15654398-42c5-d54d-a15f-baf454s2354",
    "defaultLocale": "en-US",
    "hostnames": [
        "mystore.nexway.store"
    ],
    "includeTax": true,
    "logoFavicon": "https://mywebsite.com/images/logo_favicon.png",
    "logoStore": "https://mywebsite.com/images/logo_store.png",
    "name": "My First Store",
    "saleLocales": [
        "en-US",
        "fr-FR"
    ],
    "status": "ENABLED",
    "theme": "Black theme"
}
```