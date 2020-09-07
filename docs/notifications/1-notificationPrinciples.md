
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

## List of resource notifications
* Orders
* Subscriptions

## Configure notifications
You can configure notifications via Nexway Center or via [APIs](/nexway-monetize/reference/manage-notifications).

### Create an order confirmation notification
In this example, we want to send an order confirmation notification both by email and by webhook.

The customer ID is `06874434-4d42-423e-87c0-3290862809cc`.

Use the [Create notification rules](/nexway-monetize/reference/manage-notifications/create-notifier-rules) to `POST` the following rule:
```json
{
    "customerId": "06874434-4d42-423e-87c0-3290862809cc",
    "rules": [
        {
            "resource": "Order",
            "eventType": "confirmed",
            "isActive": true,
            "emailChannel": {
                "recipients": [
                    "jdoe@com2us.com",
                    "tmonk@com2us.com"
                ]
            },
            "webHooks": {
                "urlList": [
                    {
                        "Host": "backoffice.com2us.com",
                        "Path": "/webhooks",
                        "Scheme": "https"
                    }
                ]
            }
        }
    ]
}
```

So notifications will be sent by email to jdoe@com2us.com and tmonk@com2us.com, and the same notification will appear on `https://backoffice.com2us.com/webhooks`.
As the `isActive` value is true, the rule will be executed right after the `POST` request.
