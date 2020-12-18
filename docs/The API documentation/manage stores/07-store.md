
## Store resource

A store is a point of sale that belongs to a customer (who may have more than one store).

The store resource contains at least the following:

- A name
- One or more locales (default and list of available)
- List of hostnames for store access (checkout and end-user portal)
- Tax inclusion data
- Design data: layout, theme, graphics (logo, banner, favicon, etc.)

Methods

- [post](/docs/The%20API%20Documentation/manage%20stores/01-create-store.md) - Create a store
- [get](https://api-doc.nexway.store/nexway-monetize/reference/shopper-apis/get-store?v=latest) - Get a store by ID
- [get](/docs/The%20API%20Documentation/manage%20stores/03-get-store.md) - Get a store by ID
- [get](/docs/The%20API%20Documentation/manage%20stores/02-get-store-by-hostname.md) - Get a store by hostname
- [put](/docs/The%20API%20Documentation/manage%20stores/05-update-store.md) - Update a store

### Resource

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
    "theme": "string",
    "updateDate": "date-time"
}
```

### Properties

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
|theme| 	string|Deprecated|	Optional, read only. |
