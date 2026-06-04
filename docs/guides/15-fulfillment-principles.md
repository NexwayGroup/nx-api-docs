# How fulfillment works

Fulfillment is the step in the order workflow that confirms the partner can deliver a purchased product, including triggering physical delivery via ERP integration. Confirmation can take various forms:

* license keys
* activation codes
* serial numbers
* activation links
* certificates
* or simply an acknowledgement from your server

If fulfillment cannot retrieve this confirmation, the order stalls and requires intervention from the operations team.

The fulfillment call is used to:
* Get a license key/activation code/serial number from a partner's server
* Activate a service or a license key to a partner's server
* Confirm that the partner is able to provide a purchased product

## List of fulfillment actions
Each action is associated with an order workflow event on the Nexway Monetize Platform. Some actions are only useful in a subscription context. The Nexway team configures the action-to-URL mapping during onboarding.

Action |Event on Nexway side | Endpoint called on your side | Result
------ | ------------------- | ---------------------------- | -----
Create | Order is confirmed | https://yourserver.com/licenses/new | Activate or get a license key for a product
Cancel | Order is canceled | https://yourserver.com/licenses/cancel | Cancel/Revoke a license key
Renew (subscription only) | Renewal of subscription product | https://yourserver.com/licenses/renew | Get a new key or extend key validity
Upgrade (subscription [mid-term upgrade](35-subscription-management.md)) | Upgrade of subscription product | https://yourserver.com/licenses/upgrade | Upgrade the license

## Implementation options

| You already have...                       | Recommended path                                                              |
|-------------------------------------------|-------------------------------------------------------------------------------|
| A license-issuing service or partner API  | [Custom fulfillment template](#custom-fulfillment-template)                   |
| A batch of pre-generated activation codes | Nexway off-the-shelf license key provider                                     |

**Custom fulfillment template.** If you have an existing service for issuing licenses, it can be integrated into the Monetize fulfillment via a [custom fulfillment template](#custom-fulfillment-template).

**Off-the-shelf license key provider.** Nexway offers an integrated service designed to handle batches of activation codes. You provide a batch of keys for distribution. This works well during the initial phase but requires manual effort for license package maintenance.

## What is the difference between a fulfillment call and an order notification?

|                        | Fulfillment call                                             | Order notification                                |
|------------------------|--------------------------------------------------------------|---------------------------------------------------|
| **Purpose**            | Triggers delivery of the product (license, activation, etc.) | Informs you of an order/subscription event        |
| **On success**         | License assigned, transaction finalized                      | Event delivered, no further effect                |
| **On failure**         | Order set to [`PARTIAL_COMPLETED`](40-order-processing.md), ops team alerted | Order processing not affected     |
| **Retry**              | Automatic retries                                            | Automatic retries                                 |
| **Typical use**        | Issuing licenses, activating services                        | Updating BI tools, dashboards, CRM systems        |
| **Delivery guarantee** | Required — blocks order completion                           | Fire-and-forget                                   |

For details on order notifications, see [Order notification](../notifications/05-orderNotification.md).

## Custom fulfillment template

The Nexway team can configure a fulfillment template to call your server. Nexway can adapt to fields with different names than those described below, mapping values to attributes recognized by your existing service.

### Request payload

The fulfillment request is sent as a POST HTTP request and has the following attributes:

| Property                | R/O | Type                | Description                                                                                                                    |
|-------------------------|-----|---------------------|--------------------------------------------------------------------------------------------------------------------------------|
| licenseId               | R   | UUID                | Fulfillment Id, identifier of the fulfillment process                                                                          |
| operation               | R   | string              | One of the supported operations: create, renew, upgrade, cancel                                                                         |
| checkout                | R   | object              | Has some order related attributes (see below):                                                                                 |
| → orderId                 | R   | string              |                                                                                                                                |
| → lineItemId              | R   | string              | UUID of the order line item                                                                                                    |
| → cartExternalContext     | O   | string              | base64 encoded plain json map. Taken from the external context of a shopping cart.  Used to pass customer specific parameters. Example: `eyJjdXN0b21QYXJhbSI6dHJ1ZX0` (decodes to `{"customParam":true}`) |
| → subscriptionId          | O   | string              | UUID of a subscription                                                                                                         |
| → trialContext            | O   | string              | `CREATION` or `CONVERSION`                                                                                                     |
| → price                  | R   | object              | Order price                                                                                                                     |
| -→ grossPrice            | R   | number              | Order gross price                                                                                                               |
| -→ currency              | R   | string              | 3-char currency code                                                                                                            |
| user                    | R   | object              | Buyer attributes:                                                                                                              |
| → id                      | R   | string              | End-user Id                                                                                                                    |
| → companyName             | O   | string              |                                                                                                                                |
| → companyIdentifier       | O   | string              | CNPJ or VAT number. Tax identifier                                                                                             |
| → firstName               | O   | string              |                                                                                                                                |
| → lastName                | O   | string              |                                                                                                                                |
| → email                   | R   | string              |                                                                                                                                |
| → city                    | O   | string              |                                                                                                                                |
| → zipCode                 | O   | string              |                                                                                                                                |
| → country                 | R   | string              | 2 letter ISO code                                                                                                              |
| → locale                  | R   | string              | Shopping cart locale                                                                                                           |
| product                 | R   | object              | Product related attributes:                                                                                                    |
| → id                      | R   | string              | Product Id                                                                                                                     |
| → publisherProductId      | O   | string              | Publisher/ customer specific product id (if defined)                                                                           |
| → name                    | R   | string              | Internal product name                                                                                                          |
| → externalContext         | O   | string              | Any string. Defined in the catalog                                                                                             |
| → priceFunctionParameters | O   | map(string, string) | Map of price function parameters (if defined on the product level)                                                             |
| → variables               | O   | map(string, string) | Map of variables (if defined)                                                                                                  |
| → price                  | R   | object              | Product price                                                                                                                   |
| — → grossPrice            | R   | number              | Gross price                                                                                                                     |
| — → currency              | R   | string              | 3-char currency code                                                                                                            |

### Default template

Unless customized, Nexway sends the following payload. All payload-table fields are included; optional fields are emitted only when present.

```http
POST https://yourserver.com/licenses/new
Authorization: Basic <base64(user:password)>
Content-Type: application/json
```

```json
{
  "fulfillmentId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "checkout": {
    "orderId": "ORD-2026-000123",
    "lineItemId": "11111111-2222-3333-4444-555555555555",
    "subscriptionId": "99999999-8888-7777-6666-555555555555",
    "cartExternalContext": "eyJjdXN0b21QYXJhbSI6dHJ1ZX0",
    "trialContext": "CREATION",
    "price": {
      "grossPrice": 29.99,
      "currency": "EUR"
    }
  },
  "user": {
    "id": "user-abc123",
    "email": "buyer@example.com",
    "country": "FR",
    "locale": "fr-FR",
    "firstName": "Jean",
    "lastName": "Dupont",
    "companyName": "Acme SAS",
    "companyIdentifier": "FR12345678901",
    "city": "Paris",
    "zipCode": "75001"
  },
  "product": {
    "id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
    "name": "Acme Pro Edition",
    "publisherProductId": "ACME-PRO-2026",
    "externalContext": "campaign:spring",
    "priceFunctionParameters": { "seats": "5" },
    "variables": { "region": "EU" },
    "price": {
      "grossPrice": 29.99,
      "currency": "EUR"
    }
  }
}
```

The underlying template used to render this payload, along with the full templating reference (syntax, variables, custom functions, response extraction), is in [Fulfillment templating](15a-fulfillment-templating.md#default-fulfillment-template).

### Security

- **Authentication.** Basic HTTP auth is the default. Credentials are configured per integration during onboarding.
- **TLS.** Production endpoints must use HTTPS. Self-signed certificates are accepted provided you supply your CA root.

## Response format

Your server can return any JSON shape. Nexway extracts the values it needs with JSONPath expressions configured per integration during onboarding.

### Extracted fields

| Field                   | Purpose                                                          |
|-------------------------|------------------------------------------------------------------|
| `activationCode`        | Optional — the delivered license key, activation code, or serial number |
| `activationFileContent` | Optional — file content (e.g. a certificate) to deliver alongside the key |
| `activationLink`        | Optional — URL for activating the license                        |
| `errorCode`             | Optional — a non-empty value marks the call as failed            |
| `errorMessage`          | Optional human-readable error detail                             |

### Sample response

```json
{
  "licenses": [
    {
      "key": "ABCD-1234-EFGH-5678",
      "expiresAt": "2027-06-04T00:00:00Z"
    }
  ]
}
```


### Response processing

**Success.** A 2xx response with no `errorCode` (or `errorCode` empty) is treated as success. Nexway extracts the configured fields from the response body.

**Failure.** Any of the following are treated as failed:
- An HTTP error status (4xx / 5xx). Nexway retries automatically.
- A 2xx response with a non-empty `errorCode`.

The order is set to [`PARTIAL_COMPLETED`](40-order-processing.md) from the first failure, and the operations team is alerted. Retries continue in the background. If the order cannot ultimately be fulfilled, it is canceled and the customer is reimbursed.

## See also
You may subscribe to our [notifications](../notifications/01-notificationPrinciples.md) to get other events about the order or subscription lifecycle.
