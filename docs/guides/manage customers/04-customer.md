
## Customer resource

A customer is a Nexway partner. The customer resource stores different kinds of information about the partner.

Here is a non-exhaustive list of partner-related information:

- A name
- A default email
- A physical address (country, city, street address, zip/postal code, etc.)
- Identity and access management information
- Available fulfillment templates (can be linked to some products)
- Available subscription templates (can be linked to some products)
- Unique ID to use as a reference for most other kinds of resources (Store, Product, etc.)
- Key pair used to sign and verify data

### Methods

- [get](/docs/nx-api-docs/docs/guides/manage%20customers/01-get-customer.md) - Get a customer by id
- [get](/docs/nx-api-docs/docs/guides/manage%20customers/02-get-customer-by-name.md) - Get a customer by name
- [get](/docs/nx-api-docs/docs/guides/manage%20customers/03-get-customer-by-realm-name.md) - Get a customer by realm name

### Resource

```json
{
    "additionalPaymentMethods": [
        "string"
    ],
    "addressBook": {
        "city": "string",
        "country": "string",
        "phoneNumber": "string",
        "state": "string",
        "streetAddress": "string",
        "zipCode": "int64"
    },
    "createDate": "date-time",
    "createEndUserWithoutSubscription": "boolean",
    "dbVersion": "int64",
    "defaultCustomerCareEmail": "string",
    "email": "string",
    "fulfillments": {
        "<key>": "string"
    },
    "iamClient": {
        "clientSecret": "string",
        "realmName": "string",
        "realmPublicKey": "string"
    },
    "id": "string",
    "localizedCustomerCareEmails": {
        "<key>": "string"
    },
    "modelVersion": "string",
    "name": "string",
    "promoteOneClickPayment": "boolean",
    "status": "string",
    "subscriptions": {
        "<key>": "string"
    },
    "type": "string",
    "updateDate": "date-time",
    "vatNumber": "string"
}
```

### Properties

| Name |	Type |	Description |	Additional |
| -- | -- | -- | -- |
|additionalPaymentMethods[]| 	array of string|List of payment methods specifically added on this customer|	Optional|
|addressBook| 	object| 	AddressBook| 	Optional|
|addressBook.city| 	string|Address book city||	
|addressBook.country| 	string|Address book country code iso 2||	
|addressBook.phoneNumber| 	string|Address book phone number||
|addressBook.state| 	string|Address book state|	Optional|
|addressBook.streetAddress| 	string|Address book street address||
|addressBook.zipCode| 	int64|Address book zipCode||
|createDate| 	date-time| 		|Optional|
|createEndUserWithoutSubscription| 	boolean|Create an enduser whatever the product type bought (Permanent / Subscription)|	Optional|
|dbVersion| 	int64|| 		Optional|
|defaultCustomerCareEmail| 	string|Default customer care email||
|email| 	string|Customer email||
|fulfillments| 	object|Customer fulfillments|	Optional|
|fulfillments.<key\>| 	map of string| 		|Optional|
|iamClient| 	object| 	IamClient| 	Optional|
|iamClient.clientSecret| 	string|Customer auth client secret|	Optional|
|iamClient.realmName| 	string|Customer realm name||
|iamClient.realmPublicKey 	|string|Customer auth public key|	Optional|
|id| 	string| 		|Optional|
|localizedCustomerCareEmails| 	object|Customer care email by locale|	Optional|
|localizedCustomerCareEmails.<key\>| 	map of string| 		|Optional|
|modelVersion| 	string|| 		Optional|
|name| 	string|Customer name (company name)||	
|promoteOneClickPayment| 	boolean|Promote one-click payment|	Optional|
|status| 	string|Customer status. Possible values are:`TRIAL`,`RUNNING`,`SUSPENDED`|Optional|
|subscriptions 	|object|Customer subscriptions|	Optional|
|subscriptions.<key\>| 	map of string| 		|Optional|
|type| 	string|Type of customer. Possible values are:`PUBLISHER`,`RESELLER`|	Optional, read only.|
|updateDate| 	date-time| 		|Optional|
|vatNumber| 	string|Customer vat number||
	
### Example

```json
{
    "addressBook": {
        "city": "San Francisco",
        "country": "FR",
        "phoneNumber": "415-851-2460",
        "state": "California",
        "streetAddress": "236 8th street",
        "zipCode": 94103
    },
    "defaultCustomerCareEmail": "com2us_en@nexway.com",
    "localizedCustomerCareEmails": {
        "en-US": "customercare_en@nexway.com",
        "fr-FR": "customercare_fr@nexway.com"
    },
    "email": "jdoe@com2us.com",
    "fulfillments": {
        "com2us": "com2us license key fulfillment"
    },
    "iamClient": {
        "clientSecret": "8ab4841c-7e2f-78a1-c5d4-58d525e9cfc6",
        "realmName": "com2us",
        "realmPublicKey": "FDFSDF55415HAqqs5444hghrfg4h5gj4hkjhf15dh4fdh4da854kjhlkh1b4hl4HJH"
    },
    "name": "Com2Us",
    "status": "RUNNING",
    "type": "PUBLISHER",
    "updateDate": "1479225750482",
    "vatNumber": "FR45619122-451"
}
```
