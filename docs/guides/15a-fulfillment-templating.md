# Fulfillment templating

Fulfillment templates control the HTTP call Nexway makes to a partner's server during fulfillment. A template defines the URL path, request body, headers, and how Nexway should extract values from the partner's response. Templates are configured per integration by the Nexway operations team.

For the high-level fulfillment flow and request payload field reference, see [How fulfillment works](15-fulfillment-principles.md).

## How a template is used

Each integration carries one template definition per fulfillment operation (`create`, `cancel`, `renew`, `upgrade`, `pause`, `resume`). At call time Nexway:

1. Picks the template matching the operation. If no per-operation template is defined, a fallback template is used.
2. Renders the URL path and request body against the order's data context.
3. Sends a `POST` request with the rendered body and the operation's configured headers.
4. Parses the response with the operation's JSONPath extractors and stores the extracted values on the fulfillment.

## Template configuration

A template definition has four parts, each scoped to one operation:

| Part            | Purpose                                                                                  |
|-----------------|------------------------------------------------------------------------------------------|
| `urlComplement` | The URL path appended to the partner's base URL. Supports template syntax.               |
| `bodyTemplate`  | The request body, rendered to JSON. Supports template syntax.                            |
| `httpHeaders`   | Map of request headers added on top of the integration's default headers.                |
| `responsePaths` | Map of JSONPath expressions used to extract values from the partner's response.          |

## Template syntax

Templates use Go-style `text/template` syntax. The runtime supports the constructs partners typically need:

- **Field access.** `{{.Checkout.OrderID}}` — references a value in the data context.
- **Conditional block.** `{{if eq .Operation "create"}}new{{end}}` — emits content only if the predicate is true.
- **Optional field.** `{{- with .User.FirstName -}}, "firstName": "{{.}}"{{- end -}}` — emits the block only when the field is non-empty. The dash trims surrounding whitespace.
- **Pipes.** `{{ convertToJson .Product.Variables }}` — passes a value through a function.

## Template context

The data context is the root object passed to the template. All fields are referenced with PascalCase identifiers (matching the underlying struct).

### Top level

| Field                  | Type    | Description                                                       |
|------------------------|---------|-------------------------------------------------------------------|
| `LicenseID`            | string  | Fulfillment identifier (UUID)                                     |
| `Operation`            | string  | One of `create`, `cancel`, `renew`, `upgrade`, `pause`, `resume`  |
| `OperationExecutionID` | string  | Identifier of this specific execution attempt                     |
| `RequestTimestamp`     | number  | Epoch milliseconds when the request was queued                    |
| `Checkout`             | object  | Order-level data (see below)                                      |
| `User`                 | object  | Buyer data (see below)                                            |
| `Product`              | object  | Product data (see below)                                          |
| `AdditionalData`       | map     | Arbitrary key-value pairs; values are lists of strings            |

### `Checkout`

| Field                   | Type            |
|-------------------------|-----------------|
| `OrderID`               | string          |
| `SubscriptionID`        | string          |
| `CartExternalContext`   | string          |
| `StoreExternalContext`  | string          |
| `AffiliateID`           | string          |
| `ResellerID`            | string          |
| `BillingPlanID`         | string          |
| `ProductUsageID`        | string          |
| `TrialContext`          | string          |
| `Price.GrossPrice`      | number          |
| `Price.Currency`        | string          |

### `User`

| Field          | Type   |
|----------------|--------|
| `ID`           | string |
| `Email`        | string |
| `FirstName`    | string |
| `LastName`     | string |
| `CompanyName`  | string |
| `Street`       | string |
| `City`         | string |
| `ZipCode`      | string |
| `Country`      | string |
| `Locale`       | string |

### `Product`

| Field                     | Type               |
|---------------------------|--------------------|
| `ID`                      | string             |
| `PublisherProductID`      | string             |
| `PublisherFulfillmentID`  | string             |
| `LineItemID`              | string             |
| `Name`                    | string             |
| `ExternalContext`         | string             |
| `StartTimestamp`          | number (epoch ms)  |
| `ExpirationTimestamp`     | number (epoch ms)  |
| `Quantity`                | number             |
| `Price.GrossPrice`        | number             |
| `Price.Currency`          | string             |
| `PriceFunctionParameters` | map(string,string) |
| `Variables`               | map(string,string) |
| `ActivationLink`          | string             |

### `AdditionalData`

A flat map of arbitrary keys to lists of strings. Common entries:

| Key                  | Origin                                                   |
|----------------------|----------------------------------------------------------|
| `ActivationCode`     | Activation code from a previous fulfillment (renewals)   |
| `PublisherLicenseID` | License identifier returned by the partner               |

Other keys may be present depending on the integration.

## Custom functions

| Function                     | Signature                          | Purpose                                         |
|------------------------------|------------------------------------|-------------------------------------------------|
| `convertToJson`              | `convertToJson value`              | Serializes a value to a JSON literal.           |
| `timestampToRFC3339`         | `timestampToRFC3339 epochMillis`   | Converts epoch milliseconds to ISO 8601 UTC.    |
| `default`                    | `default value fallback`           | Returns `fallback` if `value` is null or empty. |
| `eq`                         | `eq a b`                           | String equality. Used in `if`.                  |
| `ne`, `lt`, `le`, `gt`, `ge` | comparison operators               | Numeric or string comparison.                   |
| `not`, `and`, `or`           | logic operators                    | Combine boolean predicates.                     |
| `len`                        | `len value`                        | Length of a string, list, or map.               |
| `index`                      | `index collection key`             | Indexes into a list or map.                     |
| `slice`                      | `slice value start end`            | Substring or sublist.                           |
| `print`, `printf`, `println` | string formatting                  | Format helpers.                                 |

## Response extraction

Each operation defines a `responsePaths` map. Keys are extraction names; values are JSONPath expressions evaluated against the response body. Extracted values are stored on the fulfillment and (for known names) drive success or failure decisions.

### Standard extraction names

| Name                    | Purpose                                                          |
|-------------------------|------------------------------------------------------------------|
| `activationCode`        | Delivered license key, activation code, or serial number         |
| `activationFileContent` | File content (e.g. a certificate) to deliver alongside the key   |
| `activationLink`        | URL for activating the license                                   |
| `successFlag`           | Must equal `"true"` to count as success when present             |
| `errorCode`             | Non-empty value marks the call as failed                         |
| `errorMessage`          | Human-readable error detail                                      |

Any other extraction names are also captured and stored alongside the fulfillment as additional data.

### JSONPath suffix

A path ending in `+` (for example `$.licenses[0].activationCode+`) is treated as a list-extraction — Nexway captures all matching values rather than the first one.

### Response value conversion

Each extraction entry can carry an optional `conversionTemplate` — a small template applied to the extracted value before it is stored. Use this to reshape or normalize values (e.g. strip a prefix, build a URL around a code).

## Default fulfillment template

This is the template body Nexway sends by default when no custom fulfillment template is configured for a partner. The operations team uses it as a starting point when setting up new integrations or reviewing existing ones.

A rendered payload example and field definitions are in [How fulfillment works](15-fulfillment-principles.md#default-template).

```
{
  "fulfillmentId": "{{.LicenseID}}",
  "checkout": {
    "orderId": "{{.Checkout.OrderID}}",
    "lineItemId": "{{.Checkout.LineItemID}}",
    {{- with .Checkout.SubscriptionID }}
    "subscriptionId": "{{.}}",
    {{- end }}
    {{- with .Checkout.CartExternalContext }}
    "cartExternalContext": "{{.}}",
    {{- end }}
    {{- with .Checkout.TrialContext }}
    "trialContext": "{{.}}",
    {{- end }}
    "price": {
      "grossPrice": {{.Checkout.Price.GrossPrice}},
      "currency": "{{.Checkout.Price.Currency}}"
    }
  },
  "user": {
    "id": "{{.User.ID}}",
    "email": "{{.User.Email}}",
    "country": "{{.User.Country}}",
    "locale": "{{.User.Locale}}"
    {{- with .User.FirstName }},
    "firstName": "{{.}}"
    {{- end }}
    {{- with .User.LastName }},
    "lastName": "{{.}}"
    {{- end }}
    {{- with .User.CompanyName }},
    "companyName": "{{.}}"
    {{- end }}
    {{- with .User.CompanyIdentifier }},
    "companyIdentifier": "{{.}}"
    {{- end }}
    {{- with .User.City }},
    "city": "{{.}}"
    {{- end }}
    {{- with .User.ZipCode }},
    "zipCode": "{{.}}"
    {{- end }}
  },
  "product": {
    "id": "{{.Product.ID}}",
    "name": "{{.Product.Name}}"
    {{- with .Product.PublisherProductID }},
    "publisherProductId": "{{.}}"
    {{- end }}
    {{- with .Product.ExternalContext }},
    "externalContext": "{{.}}"
    {{- end }}
    {{- with .Product.PriceFunctionParameters }},
    "priceFunctionParameters": {{ convertToJson . }}
    {{- end }}
    {{- with .Product.Variables }},
    "variables": {{ convertToJson . }}
    {{- end }},
    "price": {
      "grossPrice": {{.Product.Price.GrossPrice}},
      "currency": "{{.Product.Price.Currency}}"
    }
  }
}
```

## Example: a single-endpoint integration

A partner exposes one endpoint that handles all operations, distinguishing them by URL suffix. The integration is configured with:

**`urlComplement`**

```
/licenses/{{if eq .Operation "create"}}new{{end}}{{if eq .Operation "renew"}}renew{{end}}{{if eq .Operation "cancel"}}cancel{{end}}{{if eq .Operation "upgrade"}}upgrade{{end}}
```

**`bodyTemplate`**

```
{
  "fulfillmentId": "{{.LicenseID}}",
  "orderId": "{{.Checkout.OrderID}}",
  "productCode": "{{.Product.PublisherProductID}}",
  "buyerEmail": "{{.User.Email}}"
  {{- with .AdditionalData.ActivationCode }},
  "existingActivationCode": "{{.}}"
  {{- end }}
}
```

**`responsePaths`**

| Name             | JSONPath              |
|------------------|-----------------------|
| `activationCode` | `$.result.licenseKey` |
| `errorCode`      | `$.error.code`        |
| `errorMessage`   | `$.error.message`     |
