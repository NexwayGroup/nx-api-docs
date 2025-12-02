
# How notifications work

Notifications keep you informed in real time about events which happen in the Nexway Monetize platform. A notification can be delivered via:
* Email
* HTTP webhook call (JSON format)

Note that there is no way to reply to an email notification.

## Delivery methods

### By webhook
Webhooks provide a way to deliver notifications to an external web server whenever certain actions or changes in status affect one of your resources in Nexway Monetize.
The HTTP body request is available only in JSON format.

### By email
By default, the email body will use the same JSON format as the webhook body.

## Notification subjects

All notifications are generated when the state of some domain entity changes. Currently we support notifications about:
* [Orders](05-orderNotification.md)
* [Subscriptions](10-subscriptionNotification.md)
* [End users](15-enduserNotification.md)

## Notification format

The notifications are sent in the JSON format with some common fields:

| Name | Description |
| ---- | ----------- |
| subject | Event entity: order, sbscription, endUser |
| type | Event name |
| objectId | Entity Id (orderId, subscriptionId or endUserId) |
| eventDate | Date in ISO 8601 format |
| entity | Entity object. See details in the corresponding subject type article. |

```json
{
	"subject": "Subject name",
	"type": "Event name",
	"objectId": "Subject Id",
	"eventDate": "2017-08-17T11:25:33.606Z",
  "{{entity}}": {}
}
```

## Authentication

We can authenticate with your endpoint using:
* Basic HTTP authentication header
* OAuth
* TLS 1.2+ (server or client certificate)


## Configure notifications
You can configure notifications via APIs or in the MyNexway UI. To subscribe for the notifications you need to create a receiver for your customerId and choose notification definitions.

You can get notification definitions calling
GET https://api.nexway.store/notification/definitions

## Examples

### How to receive 'order completed' notifications by email
In this example, we want to receive order confirmation notifications on 'email@domain.com' email.

The customerId is `06874434-4d42-423e-87c0-3290862809cc`.
The notificationDefinitionId is `19656f45-db84-4fa9-bd24-8917b88fb6b5`.

Create a receiver with a POST request to https://api.nexway.store/notification/receivers

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

Notifications will be sent to email@domain.com upon the successfull completion of the request.


### How to receive 'order completed' notifications by webhook

In this example, we want to receive an order confirmation notification by webhook.

The customerId is `06874434-4d42-423e-87c0-3290862809cc`.
The notificationId is `19656f45-db84-4fa9-bd24-8917b88fb6b5`.

Create a receiver with a POST request to https://api.nexway.store/notification/receivers

#### Authentication by OAuth 2.0
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

#### Authentication by a client certificate
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

As the status of the receiver is `ACTIVE`, notifications will be POSTed to `https://notifications.your-domain.com/{webhooksPath}`. The notification listening endpoint should respond with 200 or 201 HTTP status code. Otherwise the notification delivery will be failed and will be retried several times.
