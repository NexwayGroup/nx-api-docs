
# How fulfillment works

The fulfillment callback is used to:
* Get a license key/activation code/serial number from a partner's server
* Activate a license key to a partner's server

Nexway provides standard fulfillment client-side REST APIs with a predefined set of calls/actions and a common payload.

> In this documentation, some words refer the same entity: Customer, Publisher, and Partner all mean "you".

## Common payload/body "LicenseRequest"
The Nexway Monetize Platform will send a common payload called [LicenseRequest Resource](/nexway-monetize/resources/licenserequest) for all requested actions.

To ease the implementation on your side, you can see the [swagger file](http://api.staging.nexway.build/sample-fulfillment-provider/v2/api-docs/) and the associated [swagger UI](http://api.staging.nexway.build/sample-fulfillment-provider/swagger-ui.html).

> You can use the [Swagger Editor](http://editor.swagger.io/#/) to generate API server skeleton code for your language (PHP, java, etc.).

## List of actions
Each action is associated with an event on the Nexway Monetize Platform. Some actions are only useful in a subscription context.

Action |Event on Nexway side | Endpoint called on your side | Result
------ | ------------------- | ---------------------------- | -----
New | Order is confirmed | http://yourserver.com/licenses/new | Activate or get a license key for a product
Cancel | Order is canceled | http://yourserver.com/licenses/cancel | Cancel/Revoke a license key
Renew (subscription only) | Renewal of subscription product | http://yourserver.com/licenses/renew | Get a new key or extend key validity
Suspend (subscription only) | Suspension of subscription product | http://yourserver.com/licenses/suspend | Suspend/Cancel a license key
Reactivate (subscription only) | Reactivation of subscription product | http://yourserver.com/licenses/reactivate | Reactivate a license key or get a new one

## Sample fulfillment server
Nexway provides a sample fulfillment server for you to implement on your side to test expected behavior.
Documentation is available [here](/nexway-monetize/reference/fulfillment-sample-server).
 
