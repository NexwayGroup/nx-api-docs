# End-user (Buyer) Notifications

These notifications are triggered when the End-user data changes. 

## Notification Events

* **EndUser Deleted** - when an end-user account is deleted

## EndUser Deleted Payload Structure

| Name | Description | R / O |
| :--- | :---------- | :---: |
| **subject** | For order notifications the subject is `endUser` | R |
| **type** | `deleted` | R |
| **objectId** | EndUser identifier | R |
| **eventDate** | ISO date of the event | R |


