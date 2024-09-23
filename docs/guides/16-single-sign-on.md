# Single Sign-On (SSO)

Our API allows you to generate a deep link that automatically logs an end user into their portal. This enables a smooth transition from your customer portal to the Nexway-managed portal, where users can manage their subscriptions, payment methods, billing details, download invoices, and more.

## How to Create a Deep Link

To create a deep link to the home page of the end user portal, follow these instructions.

### Required Parameters:
- `enduserId`: The unique identifier of the end user.
- `baseLink`: The target URL for the end user's portal. The hostname must correspond to the store that the end user is associated with, which can be retrieved from the [EndUser resource](https://apidoc.nexway.store/api/enduser#tag/EndUser/operation/getItem). Ensure you use the specific URL linked to that end user’s store.
- `linkDuration` parameter defines the validity of the deep link, in seconds. For security purposes, it's recommended to generate short-lived links.
- `singleUse` set to true if the deeplink will be reused.

> Note: Stores may have multiple URLs, and each end user is linked to a specific one. Make sure to use the URL corresponding to the specific end user.

### Sample 
```
POST https://api.nexway.store/iam/deeplinks/
Authorization Bearer {{accessToken}}
```
```json
{
    "enduserId": "{{endUserId}}",
    "baseLink": "https://{{storeUrl}}/enduser-portal/home",
    "linkDuration": 100000,
    "singleUse": false
}
```

Response

If the deeplink is generated successfully you'l receive 201 HTTP response and Location header containing a deeplink. 

Errors

If the baseLink parameter contains an incorrect store URL (i.e., a URL that is not associated with the end user), the API will return a 400 HTTP response with the message: "baselink is not from the enduser's store." This indicates that you must retrieve the correct store URL linked to the end user from the EndUser resource.