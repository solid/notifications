# StreamingHTTPSubscription2021

The [Solid Notifications Protocol](https://solid.github.io/notifications/protocol)
defines a set of interaction patterns for agents to establish subscriptions to resources in a Solid Storage.

This specification defines a subscription type that applies these patterns to the Fetch API.

## Introduction

*This section is non-normative.*

The [Solid Notifications Protocol](https://solid.github.io/notifications/protocol)
describes a general pattern by which agents can be notified when a Solid Resource changes.
In the context of a Web Browser, the Streaming HTTP API provides a convenient mechanism for a Solid Storage
to alert a subscribing client of these changes.

This document describes a Solid Notifications subscription type that makes use of the Fetch API.

This specification is for:

* Resource server developers who wish to enable clients to listen for updates to particular resources.
* Application developers who wish to implement a client to listen for updates to particular resources.

### Terminology

**This section is non-normative.**

This document uses terminology from the Solid Notification Protocol, including "subscription API", "gateway API".
This document also uses terms from The OAuth 2.0 Authorization Framework specification,
including "resource server", "authorization server", "access token", and "client",
as well as terms from the WebSub specification, including "topic".

### Conformance



All assertions, diagrams, examples, and notes are non-normative, as are all sections explicitly marked non-normative.
Everything else is normative.

The key words “MUST” and “MUST NOT” are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14)
[[RFC2119]](https://datatracker.ietf.org/doc/html/rfc2119) [[RFC8174]](https://datatracker.ietf.org/doc/html/rfc8174) when,
and only when, they appear in all capitals, as shown here.


## StreamingHTTPSubscription2021 Type

This specification defines the StreamingHTTPSubscription2021 type for use with Solid Notifications subscriptions.
that use the Fetch API.

An StreamingHTTPSubscription2021 API MUST conform to the [Solid Notifications Protocol](https://htmlpreview.github.io/?https://github.com/solid/notifications/blob/7cbc56dd0732f1369daf216d108d0b7e41097399/eventsource-subscription-2021.html#discovery).

An StreamingHTTPSubscription2021 API SHOULD support the [Solid Notifications Features](https://htmlpreview.github.io/?https://github.com/solid/notifications/blob/7cbc56dd0732f1369daf216d108d0b7e41097399/eventsource-subscription-2021.html#notification-features).

The StreamingHTTPSubscription2021 type defines the following properties:

#### endpoint
The endpoint property is used in the body of the subscription response.
The value of this property MUST be a URI, using the `https` scheme.
A JavaScript client would use the entire value as input to the `fetch` function.

A client establishes a subscription using the StreamingHTTPSubscription2021 type by sending an authenticated subscription request
to the hypermedia API retrieved via Solid Notifications discovery.

For StreamingHTTPSubscription2021 interactions, the client sends a JSON-LD payload to the appropriate hypermedia API via POST.
The only required fields in this interaction are type and topic. The type field MUST contain the type of subscription being requested.
The topic field MUST contain the URL of the resource a client wishes to subscribe to changes.

### Subscription Example

*This section is non-normative.*

An example `POST` request using a `DPoP` bound access token is below:

```http
POST /subscription
Authorization: DPoP <token>
DPoP: <proof>
Content-Type: application/ld+json
```
```jsonld
{
  "@context": ["https://www.w3.org/ns/solid/notification/v1"],
  "type": "StreamingHTTPSubscription2021",
  "topic": "https://storage.example/resource",
  "state": "opaque-state",
  "expiration": "2021-12-23T12:37:15Z",
  "rate": "PT10s"
}
```
> POST request including type and topic targeting the Notification Subscription API.

A successful response will contain a URL to the subscription API that can be used directly with a JavaScript client.

```http
Content-Type: application/ld+json
```
```jsonld
{
  "@context": "https://www.w3.org/ns/solid/notification/v1",
  "type": "StreamingHTTPSubscription2021",
  "endpoint": "https://fetch.example/?auth=Ys3KiUq"
}
```
> Response to the POST request including type and endpoint.

In JavaScript, a client can use the data in the response to establish a connection to the Fetch API.
And define how to handle notifications

```js
 (async function () {
        const request = await fetch('https://fetch.example/?auth=Ys3KiUq')
        const reader = request.body.getReader()
        const decoder = new TextDecoder()
        reader.read().then(function handle({done, value}) {
            console.log(decoder.decode(value))
            return reader.read().then(handle)
        })
      })()

```

## Authentication and Authorization

Streaming HTTP Subscription has the advantage of being able to authenticate with the notification endpoint
not only the subscription endpoint. The same *access token* can be used with both endpoints.
This doesn't just rely on the notification endpoint being a [Capability URL](https://www.w3.org/TR/capability-urls/)

As described by the Solid Notifications Protocol section on Authorization,
the Streaming HTTP subscription API requires authorization and follows the guidance of the Solid Protocol
sections on Authentication and Authorization [SOLID-PROTOCOL](https://solidproject.org/TR/protocol).

It is beyond the scope of this document to describe how a client fetches an access token.
Solid-OIDC is one example of an authentication mechanism that could be used with Solid Notifications [SOLID-OIDC](https://solid.github.io/solid-oidc/).

