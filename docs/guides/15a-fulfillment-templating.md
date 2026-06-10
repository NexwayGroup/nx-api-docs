# Fulfillment templating

Fulfillment templates control the HTTP call Nexway makes to a partner's server during fulfillment. A template defines the URL path, request body, headers, and how Nexway should extract values from the partner's response. Templates are configured per integration by the Nexway operations team.

For the high-level fulfillment flow and request payload field reference, see [How fulfillment works](15-fulfillment-principles.md).

## How a template is used

Each integration carries one template definition per fulfillment operation (`create`, `cancel`, `renew`, `upgrade`) or a single `fallback` template for all operations. At call time Nexway:

1. Picks the template matching the operation. If no per-operation template is defined, a `fallback`  template is used.
2. Renders the URL path and request body against the order's data context.
3. Sends a `POST` request with the rendered body and the operation's configured headers.
4. Parses the response with the operation's JSONPath extractors and stores the extracted values on the fulfillment.

## Template configuration

A template definition has five parts, each scoped to one operation:

| Part                  | Purpose                                                                                  |
|-----------------------|------------------------------------------------------------------------------------------|
| `urlComplement`       | The URL path appended to the partner's base URL. Supports template syntax.               |
| `bodyTemplate`        | The request body, rendered to JSON. Supports [template syntax](#template-syntax).        |
| `httpHeaders`         | Map of request headers added on top of the integration's default headers.                |
| `responsePaths`       | Map of JSONPath expressions used to extract values from the partner's response.          |
| `signatureDefinition` | Optional HMAC-SHA256 signing config. See [Outbound request signing](#outbound-request-signing). |

## Template syntax

Templates use Go-style `text/template` syntax. The runtime supports the constructs partners typically need:

- **Field access.** `{{.Checkout.OrderID}}` — references a value in the data context.
- **Conditional block.** `{{if eq .Operation "create"}}new{{end}}` — emits content only if the predicate is true.
- **Optional field.** `{{- with .User.FirstName -}}, "firstName": "{{.}}"{{- end -}}` — emits the block only when the field is non-empty. The dash trims surrounding whitespace.
- **Pipes.** `{{ convertToJson .Product.Variables }}` — passes a value through a function.

## Request payload

These are all the fields available in the template data context. Field names here match template variable names directly (e.g. `{{.Checkout.OrderID}}`). The [default template](#default-fulfillment-template) includes all required fields and the standard optional ones; custom templates can include any subset.

| Field                     | R/O | Type                | Description                                                                                                                    |
|---------------------------|-----|---------------------|--------------------------------------------------------------------------------------------------------------------------------|
| LicenseID                 | R   | UUID                | Fulfillment identifier                                                                                                         |
| OperationExecutionID      | O   | string              | Identifier of this specific execution attempt                                                                                  |
| RequestTimestamp          | O   | number              | Epoch milliseconds when the request was queued                                                                                 |
| Operation                 | R   | string              | One of: `create`, `cancel`, `renew` (subscription only), `upgrade` (subscription only)                                        |
| Checkout                  | R   | object              | Order-level data                                                                                                               |
| → OrderID                 | R   | string              |                                                                                                                                |
| → LineItemID              | R   | string              | UUID of the order line item                                                                                                    |
| → SubscriptionID          | O   | string              | UUID of a subscription                                                                                                         |
| → CartExternalContext     | O   | string              | Base64-encoded JSON map from the shopping cart's external context. Used to pass customer-specific parameters. Example: `eyJjdXN0b21QYXJhbSI6dHJ1ZX0` (decodes to `{"customParam":true}`) |
| → TrialContext            | O   | string              | `CREATION` or `CONVERSION`                                                                                                     |
| → Price                   | R   | object              | Order price                                                                                                                    |
| –→ GrossPrice             | R   | number              |                                                                                                                                |
| –→ Currency               | R   | string              | 3-char currency code                                                                                                           |
| –→ DiscountedPrice        | O   | object              | Present when a discount was applied                                                                                            |
| ––→ DiscountedGrossPrice  | O   | number              | Post-discount gross price                                                                                                      |
| ––→ DiscountedNetPrice    | O   | number              |                                                                                                                                |
| ––→ DiscountRate          | O   | number              | Discount percentage                                                                                                            |
| User                      | R   | object              | Buyer attributes                                                                                                               |
| → ID                      | R   | string              | End-user Id                                                                                                                    |
| → Email                   | R   | string              |                                                                                                                                |
| → FirstName               | O   | string              |                                                                                                                                |
| → LastName                | O   | string              |                                                                                                                                |
| → CompanyName             | O   | string              |                                                                                                                                |
| → CompanyIdentifier       | O   | string              | CNPJ or VAT number. Tax identifier                                                                                             |
| → Street                  | O   | string              |                                                                                                                                |
| → City                    | O   | string              |                                                                                                                                |
| → ZipCode                 | O   | string              |                                                                                                                                |
| → Country                 | R   | string              | 2-letter ISO code                                                                                                              |
| → Locale                  | R   | string              | Shopping cart locale                                                                                                           |
| Product                   | R   | object              | Product attributes                                                                                                             |
| → ID                      | R   | UUID                | Product Id                                                                                                                     |
| → Name                    | R   | string              | Internal product name                                                                                                          |
| → PublisherProductID      | O   | string              | Publisher/customer-specific product id (if defined)                                                                            |
| → PublisherFulfillmentID  | O   | string              | Publisher-side fulfillment identifier                                                                                          |
| → ExternalContext         | O   | string              | Any string. Defined in the catalog                                                                                             |
| → StartTimestamp          | O   | number              | Epoch milliseconds                                                                                                             |
| → ExpirationTimestamp     | O   | number              | Epoch milliseconds                                                                                                             |
| → Quantity                | O   | number              |                                                                                                                                |
| → ActivationLink          | O   | string              |                                                                                                                                |
| → PriceFunctionParameters | O   | map(string, string) | Map of price function parameters (if defined on the product level)                                                             |
| → Variables               | O   | map(string, string) | Map of variables (if defined)                                                                                                  |
| → Price                   | R   | object              | Product price                                                                                                                  |
| –→ GrossPrice             | R   | number              |                                                                                                                                |
| –→ Currency               | R   | string              | 3-char currency code                                                                                                           |
| –→ DiscountedPrice        | O   | object              | Present when a discount was applied                                                                                            |
| ––→ DiscountedGrossPrice  | O   | number              | Post-discount gross price                                                                                                      |
| ––→ DiscountedNetPrice    | O   | number              |                                                                                                                                |
| ––→ DiscountRate          | O   | number              | Discount percentage                                                                                                            |
| ––→ DiscountID            | O   | string              |                                                                                                                                |
| ––→ DiscountCode          | O   | string              |                                                                                                                                |
| AdditionalData            | O   | map(string, list)   | Arbitrary key-value pairs; values are lists of strings. Common keys: `ActivationCode` (activation code from a previous fulfillment, used in renewals), `PublisherLicenseID` (license identifier returned by the partner). Other keys may be present depending on the integration. |

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
| `signature`                  | `{{ signature }}`                  | Emits the computed HMAC-SHA256 signature as lowercase hex. Only available when `signatureDefinition.injectInBody = true`. |

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

---

## Outbound request signing

If your server needs to verify that a fulfillment call originated from Nexway, you can enable HMAC-SHA256 request signing. Nexway computes a signature over a declared set of request fields and delivers it as an HTTP header, a value in the request body, or both.

### 1. Generate a signing key

A single signing key is held per integration. The 256-bit secret is generated server-side and returned **once** in the creation response — it is never returned again.

| Method   | Path                      | Purpose                                                                                  |
|----------|---------------------------|------------------------------------------------------------------------------------------|
| `POST`   | `/signing-keys`           | Generate a new key, discarding any existing one. Returns the secret plaintext once.      |
| `GET`    | `/signing-keys`           | Returns key metadata (`id`, `algorithm`, `createdAt`). Never returns the secret.         |
| `DELETE` | `/signing-keys`           | Revoke the active key.                                                                   |
| `POST`   | `/signing-keys/verify`    | Self-test endpoint — see [Verifying during integration](#verifying-during-integration).  |

Store the secret securely on first creation. To rotate, call `POST /signing-keys` again — the previous secret is discarded immediately and all subsequent fulfillment calls use the new one.

### 2. Configure signing on an operation

Add a `signatureDefinition` block to the relevant operation in the integration's template definition:

| Field           | Type           | Description                                                                                     |
|-----------------|----------------|-------------------------------------------------------------------------------------------------|
| `enabled`       | boolean        | Must be `true` to activate signing for this operation.                                          |
| `signedFields`  | list of string | JSONPath expressions into the fulfillment request (e.g. `$.checkout.orderId`). Sorted and deduplicated on save. |
| `headerName`    | string         | Header to inject the signature into. Defaults to `X-Nexway-Signature`. Leave blank to suppress. |
| `injectInBody`  | boolean        | When `true`, the `{{ signature }}` token in `bodyTemplate` is replaced with the hex signature.  |

At least one of `headerName` (non-blank) or `injectInBody = true` must be set when `enabled = true`. Both can be active simultaneously — the same hex value is emitted in each location.

**Validation on save:**
- `enabled = true` with an empty `signedFields` list is rejected.
- `injectInBody = true` without a `{{ signature }}` token in `bodyTemplate` is rejected.
- Each path in `signedFields` must be a valid JSONPath expression that resolves to a scalar.

### Signature algorithm

Both Nexway and your server must produce identical results from the same inputs. The algorithm is fixed at HMAC-SHA256.

**Inputs:** the fulfillment request, the operation's `signedFields` list (in stored order — already sorted), the HMAC secret.

**Steps:**

1. **Resolve** each JSONPath in `signedFields` against the fulfillment request JSON.
2. **Stringify** each resolved value:
   - String → raw UTF-8, no quoting.
   - Integer → decimal with no leading zeros (`3`).
   - Decimal with no fractional part → integer form (`3.0` → `"3"`).
   - Decimal with fractional part → shortest form, no trailing zeros (`1.50` → `"1.5"`).
   - Boolean → `"true"` or `"false"` (lowercase).
   - `null` or missing path → empty string `""`.
3. **Build canonical input** — a compact JSON object whose keys are the JSONPath strings (in stored sort order) and whose values are the stringified strings from step 2. No insignificant whitespace. UTF-8, no BOM.
4. **Compute** `HMAC-SHA256(secretBytes, canonicalInputBytes)`. The secret is used as raw UTF-8 bytes.
5. **Encode** the 32-byte digest as 64-character **lowercase hex**.

**Reference test vector:**

Request:
```json
{
  "checkout": { "orderId": "ORD-42" },
  "product":  { "publisherProductId": "PRD-9", "quantity": 3 }
}
```

`signedFields` (after save-time sort):
```
["$.checkout.orderId", "$.product.publisherProductId", "$.product.quantity"]
```

Canonical input (exact bytes, no whitespace):
```
{"$.checkout.orderId":"ORD-42","$.product.publisherProductId":"PRD-9","$.product.quantity":"3"}
```

With secret `s3cret`:
```
X-Nexway-Signature: a66ccb600993e538aa50cc7b612785b8919bae518242dbd96c8fde8e4558cc9b
```

### Verifying during integration

Use `POST /signing-keys/verify` to test your implementation without triggering a real fulfillment:

**Request:**

```json
{
  "licenseProviderDefinitionId": "<your-integration-id>",
  "operation": "create",
  "operationRequest": { ... },
  "expectedSignature": "<your-computed-hex>"
}
```

**Response:**

```json
{
  "canonicalInput": "{\"$.checkout.orderId\":\"ORD-42\",...}",
  "signature": "a66ccb600993e538aa50cc7b612785b8919bae518242dbd96c8fde8e4558cc9b",
  "match": true
}
```

`match` is only present when `expectedSignature` was supplied. Use `canonicalInput` to diff your canonicalization against Nexway's if signatures diverge.

### Security notes

- The secret is returned **once** on key creation and never again — store it immediately.
- A missing signing key while `enabled = true` causes a **permanent error** on the fulfillment call; no retry is attempted.
- Keys are scoped per customer — one key cannot be used for another customer's fulfillments.
- The secret is never included in logs, traces, or error messages.
