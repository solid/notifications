# Update Delta

## 1. Abstract
When an RDF resource is updated on a storage server, a notification is sent to a subscribing client, but this notification does not tell the client how the resource was updated. As a result, the client must make another request to the storage server to retieve the document and reconcile its updates. Update Delta is a notifications feature that tells the client change, or delta between the document pre-update and post-update in the notification activity.

## 2. Status of the Document
TODO

## 3. Notification Feature
Update Delta is a notification feature. As such, any compliant storage server MUST list it among other features during the notification discover phase as the URL `http://www.w3.org/ns/solid/notifications#updateDelta`.

For example, a compliant storage server could have a document like the following:

```turtle
@prefix notify: <http://www.w3.org/ns/solid/notifications#> .
<>
  notify:notificationChannel <#websocketNotification> .
<#websocketNotification>
  a notify:WebSocketSubscription2021 ;
  notify:subscription <https://websocket.example/subscription> ;
  notify:features notify:state, notify:rate, notify:expiration, notify:updateDelta .
```

## 4. Update Activity

Any Update activity (including updates to containers) MUST include the [`result`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-result) field.

> Is the `result` field the most appropriate one for this? I didn't see an ActivityStreams property that was really appropriate. Should we make a new property?

The `result` field MUST be mapped to [`Add` type](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-add) and/or [`Remove` type](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-remove) activities.

The `object` field of the `Add` and `Remove` activities MUST map to a graph distinct from the default graph.

Each graph referenced by the `Add` and `Remove` activities must contain the respective triples that were added and removed.


In this example, the triple `<http://example.com/prince> <http://xmlns.com/foaf/0.1/name> "Prince Rogers Nelson"^^xsd:string` was removed, and the triple `<http://example.com/prince> <http://xmlns.com/foaf/0.1/name> "Ƭ̵̬̊"^^xsd:string` was added.

```json
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
  "published":"2021-08-05T01:01:49.550044Z",
  "result": [
    {
      "type": "Remove",
      "object": {
        "@id": "<Graph1Name>",
        "@graph": [
          {
            "@id": "http://example.com/prince",
            "http://xmlns.com/foaf/0.1/name": "Prince Rogers Nelson"
          }
        ]
      }
    },
    {
      "type": "Add",
      "object": {
        "@id": "<Graph2Name>",
        "@graph": [
          {
            "@id": "http://example.com/prince",
            "http://xmlns.com/foaf/0.1/name": "Ƭ̵̬̊"
          }
        ]
      }
    }
  ]
}
```

> Alternatively, we could create some terms outside of Activity Streams. The example would look like this:

```json
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
  "published":"2021-08-05T01:01:49.550044Z",
  "added": {
    "@id": "<Graph1Name>",
    "@graph": [
      {
        "@id": "http://example.com/prince",
        "http://xmlns.com/foaf/0.1/name": "Prince Rogers Nelson"
      }
    ]
  },
  "removed": {
    "@id": "<Graph2Name>",
    "@graph": [
      {
        "@id": "http://example.com/prince",
        "http://xmlns.com/foaf/0.1/name": "Ƭ̵̬̊"
      }
    ]
  }
}
```