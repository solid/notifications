# Notification Content 2022

## 1. Abstract
The notificaions protocol requires that:

> The content of a Solid Notification is defined in terms of a Activity Streams 2.0 [ACTIVITYSTREAMS-CORE]. As such, JSON-LD 1.0 is a required baseline serialization format.

However, this is still a broad perscription that causes implementation details to be up to interpretation. Subscribing clients want to be sure that the content of a notification is the same for each action. Notification-Content-2022 defines the exact content that should be expected from each action on a Solid Pod.

## 2. Status of the Document

TODO

## 3. Notification Feature

> This is proposed as a feature in this spec, but it could also be easily inserted into the main protocol.

Notification Content 2022 is a notification feature. As such, any compliant storage server MUST list it among other features during the notification discover phase as the URL `http://www.w3.org/ns/solid/notifications#NotificationContent2022`.

For example, a compliant storage server could have a document like the following:

```turtle
@prefix notify: <http://www.w3.org/ns/solid/notifications#> .

<>
  notify:notificationChannel <#websocketNotification> .

<#websocketNotification>
  a notify:WebSocketSubscription2021 ;
  notify:subscription <https://websocket.example/subscription> ;
  notify:features notify:state, notify:rate, notify:expiration, notify:NotificationContent2022 .
```

## 4. Notifications for Events
Each time an event happens on a storage server, one or more notifications will be sent to the subscribing client in the following formats.

A notification activity MUST include a [`type`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-type) field mapped to an [activity type](https://www.w3.org/TR/activitystreams-vocabulary/#types).

A notification activity SHOULD include an [`id`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-id) field.

If the subscription was initiated by an authenticated WebId, a notification activity MUST include an [`actor`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-actor) field mapped to the the WebId that initiated the subscription.

A notification activity MUST include an [`object`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-object) field mapped to a summary of the resource or container for which the notification was triggered. That summary MUST include an [`id`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-id) field containing the URI of the resource or container, and a [`type`](https://www.w3.org/TR/activitystreams-vocabulary/#types) field containing the types for the resource or container.

A notification activity MUST have a [`published`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-published) field.

A notification activity MAY have additional fields, especially if the storage server supports other notification features.

### 4.1 Resource is Created
When a resource is created in a storage server's container, a notification MUST be sent to all clients that subscribe to the resource's container. The activity's type MUST be `Update`.

```
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://www.w3.org/ns/solid/notification/v1"
  ],
  "id":"urn:uuid:<uuid>",
  "type": [
    "Update"
  ],
  "actor": [
    "<WebID>"
  ],
  "object": {
    "id":"https://example.org/<user>/some-container/",
     "type": [
      "http://www.w3.org/ns/ldp#BasicContainer",
      "http://www.w3.org/ns/ldp#Resource"
    ]
  },
  "published":"2021-08-05T01:01:49.550044Z"
}
```

### 4.2 Resource is Removed
When a resource is removed in a storage server's container, a notification MUST be sent to all clients that subscribe to the resource's container. The activity's type MUST be `Update`.

```
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://www.w3.org/ns/solid/notification/v1"
  ],
  "id":"urn:uuid:<uuid>",
  "type": [
    "Update"
  ],
  "actor": [
    "<WebID>"
  ],
  "object": {
    "id":"https://example.org/<user>/some-container/",
     "type": [
      "http://www.w3.org/ns/ldp#BasicContainer",
      "http://www.w3.org/ns/ldp#Resource"
    ]
  },
  "published":"2021-08-05T01:01:49.550044Z"
}
```

A notification MUST also be sent to all clients that subscribe to the resource itself. The activity's type MUST be `Delete`.

```
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://www.w3.org/ns/solid/notification/v1"
  ],
  "id":"urn:uuid:<uuid>",
  "type": [
    "Delete"
  ],
  "actor": [
    "<WebID>"
  ],
  "object": {
    "id":"https://example.org/<user>/some-container/resource1.ttl",
     "type": [
      "http://www.w3.org/ns/ldp#Resource"
    ]
  },
  "published":"2021-08-05T01:01:49.550044Z"
}
```

### 4.3 Resource is Updated

When an RDF resource is updated on a storage server, a notification MUST be sent to all clients that subscribe to the resource. The activity's type MUST be `Update`.

```
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://www.w3.org/ns/solid/notification/v1"
  ],
  "id":"urn:uuid:<uuid>",
  "type": [
    "Update"
  ],
  "actor": [
    "<WebID>"
  ],
  "object": {
    "id":"https://example.org/<user>/some-container/resource1.ttl",
     "type": [
      "http://www.w3.org/ns/ldp#Resource"
    ]
  },
  "published":"2021-08-05T01:01:49.550044Z"
}
```

### 4.4 Access control rules are changed
> Should this be included? Perhaps it is unneeded if you subscribe to the acl document.