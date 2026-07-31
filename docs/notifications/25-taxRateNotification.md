# Tax Rate Notifications

This section describes notifications sent when a tax (VAT) rate is created, updated, or deleted on the Nexway Monetize platform.

## Overview

Nexway Monetize allows merchants to configure tax rates per country and region. Whenever a tax rate is created, updated, or deleted, a notification is sent to the configured receiver. The specific operation is encoded in the top-level `type` field.

## Event list

* **Tax Rate Created** — `type=created`
* **Tax Rate Updated** — `type=updated`
* **Tax Rate Deleted** — `type=deleted`

## List of fields

<!-- GEN:notification:TaxRateNotification subject="`tax_rate`" type="`created` / `updated` / `deleted` — see [Event list](#event-list)" objectId="Tax rate identifier" -->
| Field | Type | Description | R/O |
|---|---|---|---|
| subject | string | `tax_rate` | R |
| type | string | `created` / `updated` / `deleted` — see [Event list](#event-list) | R |
| objectId | string | Tax rate identifier | R |
| eventDate | string (date-time) | ISO 8601 timestamp | R |
| vatRate | taxManager.TaxRate | Tax rate details object | R |
| → failed | boolean | `true` if the operation failed; `false` if it succeeded | O |
| → iso2CountryCode | string | ISO 3166-1 alpha-2 country code, e.g. `FR`, `CA` | R |
| → regionCode | string | Region or province code, e.g. `CA-QC`. Present only if the rate applies to a specific region | O |
| → taxRates | taxManager.TaxRateDetail[] | Array of tax rate entries. For multi-rate countries the array contains one entry per tax category | R |
| → → value | number | Tax rate as a decimal, e.g. `0.05` for 5% | R |
| → → taxCategory | string | Tax category identifier, e.g. `STANDARD`, `FEDERAL`, `STATE`, `VAT` | R |
| → → taxName | string | Human-readable name of the tax, e.g. `Federal Tax`, `QST` | O |
<!-- /GEN:notification:TaxRateNotification -->

## Examples

### Tax Rate Created

```json
{
  "subject": "tax_rate",
  "type": "created",
  "objectId": "83353aee-427f-473d-a628-55727f590641",
  "eventDate": "2026-05-04T14:55:34Z",
  "vatRate": {
    "failed": false,
    "iso2CountryCode": "VG",
    "regionCode": "VG-TEST",
    "taxRates": [
      {
        "value": 0.05,
        "taxCategory": "STANDARD"
      }
    ]
  }
}
```

### Tax Rate Updated

```json
{
  "subject": "tax_rate",
  "type": "updated",
  "objectId": "11109753-db0f-435b-abf4-5119a6af3ab0",
  "eventDate": "2026-05-04T16:00:00Z",
  "vatRate": {
    "failed": false,
    "iso2CountryCode": "FR",
    "taxRates": [
      {
        "value": 0.08,
        "taxCategory": "STANDARD"
      }
    ]
  }
}
```

### Tax Rate Deleted

```json
{
  "subject": "tax_rate",
  "type": "deleted",
  "objectId": "83353aee-427f-473d-a628-55727f590641",
  "eventDate": "2026-05-04T14:55:34Z",
  "vatRate": {
    "failed": false,
    "iso2CountryCode": "VG",
    "regionCode": "VG-TEST",
    "taxRates": [
      {
        "value": 0.05,
        "taxCategory": "STANDARD"
      }
    ]
  }
}
```

## Notes

- The `vatRate.taxRates` array contains the full set of rates for the given country/region in this operation, not a diff.
- For countries with multiple tax categories (e.g. federal and state), each category appears as a separate entry in `taxRates`.
