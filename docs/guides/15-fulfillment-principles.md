# How fulfillment works

Fulfillment plays a crucial role in the order processing workflow. Its primary function is to secure a unique product key or similar confirmation of a digital sale from the partner. This confirmation can take various forms, including license keys, activation codes, serial numbers, activation links, certificates or just an acknowledgement received from the partner's server. If fulfillment encounters an issue retrieving this confirmation, the corresponding order will stall and require intervention from the operations team.

The fulfillment call is used to:
* Get a license key/activation code/serial number from a partner's server
* Activate a service or a license key to a partner's server
* Confirm that the partner is able to provide a purchased product

If you have an existing service for issuing licenses it can be integrated into the Monetize fulfillment via custom fulfillment template (see below).

If you don't have such a service, Nexway provides a sample fulfillment server for you to implement on your side to test expected behavior. Please ask your account manager.

Nexway also offers an integrated off-the-shelf license key provider service designed to handle batches of activation codes. You can provide a batch of keys for distribution. This service can function effectively during the initial phase and facilitate key distribution. Nevertheless, it requires manual effort for license package maintenance, which may not be the most efficient solution for many partners. Therefore, opting for a dedicated fulfillment service is often the preferred choice to streamline the fulfillment process.

## List of fulfillment actions
Each action is associated with an order workflow event on the Nexway Monetize Platform. Some actions are only useful in a subscription context. We can map the action to a specific URL of your server.

Action |Event on Nexway side | Endpoint called on your side | Result
------ | ------------------- | ---------------------------- | -----
Create | Order is confirmed | http://yourserver.com/licenses/new | Activate or get a license key for a product
Cancel | Order is canceled | http://yourserver.com/licenses/cancel | Cancel/Revoke a license key
Renew (subscription only) | Renewal of subscription product | http://yourserver.com/licenses/renew | Get a new key or extend key validity
Upgrade (subscription [mid-term upgrade](35-subscription-managment.md)) | Upgrade of subscription product | http://yourserver.com/licenses/upgrade | Upgrade the license

## Fulfillment request payload
Nexway can configure the payload sent to the partner's server to a certain extent. The fulfillment request is sent as a POST HTTP request and has the following attributes:

| Property                | R/O | Type                | Description                                                                                                                    |
|-------------------------|-----|---------------------|--------------------------------------------------------------------------------------------------------------------------------|
| licenseId               | R   | UUID                | Fulfillment Id, identifier of the fulfillment process                                                                          |
| operation               | R   | string              | One of the supported operations: create, renew, upgrade, cancel                                                                         |
| checkout                | R   | object              | Has some order related attributes (see below):                                                                                 |
| ↳ orderId                 | R   | string              |                                                                                                                                |
| ↳ lineItemId              | R   | string              | UUID of the order line item                                                                                                    |
| ↳ cartExternalContext     | O   | string              | base64 encoded plain json map. Taken from the external context of a shopping cart.  Used to pass customer specific parameters. Example: eyJjdXN0b21QYXJhbSI6dHJ1ZX0 |
| ↳ subscriptionId          | O   | string              | UUID of a subscription                                                                                                         |
| ↳ trialContext            | O   | string              | CREATION\|CONVERSION                                                                                                           |
| user                    | R   | object              | Buyer attributes:                                                                                                              |
| ↳ id                      | R   | string              | End-user Id                                                                                                                    |
| ↳ companyName             | O   | string              |                                                                                                                                |
| ↳ companyIdentifier       | O   | string              | CNPJ or VAT number. Tax identifier                                                                                             |
| ↳ firstName               | O   | string              |                                                                                                                                |
| ↳ lastName                | O   | string              |                                                                                                                                |
| ↳ email                   | R   | string              |                                                                                                                                |
| ↳ city                    | O   | string              |                                                                                                                                |
| ↳ zipCode                 | O   | string              |                                                                                                                                |
| ↳ country                 | R   | string              | 2 letter ISO code                                                                                                              |
| ↳ locale                  | R   | string              | Shopping cart locale                                                                                                           |
| product                 | R   | object              | Product related attributes:                                                                                                    |
| ↳ id                      | R   | string              | Product Id                                                                                                                     |
| ↳ publisherProductId      | O   | string              | Publisher/ customer specific product id (if defined)                                                                           |
| ↳ name                    | R   | string              | Internal product name                                                                                                          |
| ↳ externalContext         | O   | string              | Product external context. Defined in the catalog                                                                               |
| ↳ priceFunctionParameters | O   | map(string, string) | Map of price function parameters (if defined on the product level)                                                             |
| ↳ variables               | O   | map(string, string) | Map of variables (if defined)                                                                                                  |

Nexway has the capability to initiate a customized call to the partner's server, likely including fields with different names but similar information to those mentioned above. We can create a dedicated fulfillment template that will effectively map these values to attributes recognized by your existing service.

Nexway provides standard fulfillment client-side HTTP calls with a predefined set of calls/actions and a common payload.

## Sample fulfillment call
Nexway team can configure the following fulfillment template to call your server:

POST http://yourserver.com/licenses/new
Basic HTTP Auth
```json
{
 "fulfillmentId": "<<licenseId>>",
 "orderId": "<<orderId>>",
 "cartExternalContext": "<<cartExternalContext>>",
 "productId": "<<publisherProductId>>",
 "subscriptionId": "<<subscriptionId>>"
}
```

## See also
You may subscribe to our [notifications](../notifications/01-notificationPrinciples.md) to get other events about the order or subscription lifecycle.
