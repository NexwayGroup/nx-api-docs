
# How notifications works

Notifications keep you informed in real time about events that happen on the Nexway Monetize platform.

The Nexway Monetize platform has two notification mechanisms:
* Email
* HTTP REST webhook (JSON format)

Note that there is no way to reply to either an email or a webhook notification.

## Methods

### By webhook
Webhooks provide a way to deliver notifications to an external web server whenever certain actions or changes in status affect one of your resources in Nexway Monetize.
The HTTP body request is available only in JSON format.

### By email
By default, the email body will use the same JSON format as the webhook body.

## Authentication

* TLS 1.2+ (server or client) is available.


## Configure notifications
You can configure notifications via APIs.

### How to receive order confirmation notification by email.
In this example, we want to receive an order confirmation notification by email.

The customerId is `06874434-4d42-423e-87c0-3290862809cc`.

The notificationId is `19656f45-db84-4fa9-bd24-8917b88fb6b5`.

POST receiver request on https://api.staging.nexway.build/notification/receivers
```json
{
  "customerId": "83b3d537-3687-4814-bdfc-6d3901dd2011",
  "name": "My Order Confirmation notification by email",
  "status": "ACTIVE",
  "targetedCustomerIds": [
    "83b3d537-3687-4814-bdfc-6d3901dd2011"
  ],
  "notificationDefinitionIds": [
    "19656f45-db84-4fa9-bd24-8917b88fb6b5"
  ],
  "emails": [
    "email@domain.com"
  ]
}
```

So notifications will be sent by email to email@domain.com.
As the status is `ACTIVE`, the notification receiver will start sending messages right after the `POST` request execution.

### How to receive order confirmation notification by webhook

In this example, we want to receive an order confirmation notification by webhook.

The customerId is `06874434-4d42-423e-87c0-3290862809cc`.

The notificationId is `19656f45-db84-4fa9-bd24-8917b88fb6b5`.

POST receiver request on https://api.staging.nexway.build/notification/receivers

* #### auth by oauth2
```json
{
  "customerId": "83b3d537-3687-4814-bdfc-6d3901dd2011",
  "name": "My Order Confirmation notification by webhook",
  "status": "ACTIVE",
  "targetedCustomerIds": [
    "83b3d537-3687-4814-bdfc-6d3901dd2011"
  ],
  "notificationDefinitionIds": [
    "19656f45-db84-4fa9-bd24-8917b88fb6b5"
  ],
  "url": "https://notifications.your-domain.com/{webhooksPath}",
  "httpClientConfiguration": {
    "clientCredentialOauth2Config": {
      "clientId": "client123456789",
      "clientSecret": "secret123456789",
      "tokenUrl": "https://auth.your-domain.com/token",
      "scopes": [
        "provided"
      ],
      "oauth2Type": "BASIC"
    }
  }
}
```

* #### auth by TLS
```json
{
  "customerId": "83b3d537-3687-4814-bdfc-6d3901dd2011",
  "name": "My Order Confirmation notification by webhook",
  "status": "ACTIVE",
  "targetedCustomerIds": [
    "83b3d537-3687-4814-bdfc-6d3901dd2011"
  ],
  "notificationDefinitionIds": [
    "19656f45-db84-4fa9-bd24-8917b88fb6b5"
  ],
  "url": "https://notifications.your-domain.com/{webhooksPath}",
  "httpClientConfiguration": {
    "tlsConfiguration": {
      "tlsAuthMode": "CLIENT",
      "clientCertificates": "-----BEGIN CERTIFICATE-----\n**********\n-----END CERTIFICATE-----",
      "privateKey": "-----BEGIN PRIVATE KEY-----\n**********\n-----END PRIVATE KEY-----",
      "serverCACertificates": "-----BEGIN CERTIFICATE-----\n**********\n-----END CERTIFICATE-----"
    }
  }
}
```

So notifications will appear on `https://notifications.your-domain.com/{webhooksPath}`.
As the status is `ACTIVE`, the notification receiver will start sending messages right after the `POST` request execution.
