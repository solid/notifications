# Solid Notifications

This repository hosts the technical reports for Solid Notifications.

These reports are incubated by the [Solid Notifications Panel](https://github.com/solid/notifications-panel) for inclusion in the [Solid Technical Reports](https://solidproject.org/TR/). The reports are within the scope of the [Notifications Panel Charter](https://github.com/solid/process/blob/main/notifications-panel-charter.md).

## Technical Reports
* [Solid Notifications Protocol](https://solid.github.io/notifications/protocol) Editor's Draft.

### Subscription Types

The Solid Notification Protocol makes it possible to define any number of subscription types.
The list of subscription types below does not intend to be comprehensive. This list is intended
to be useful to application developers looking for existing subscription types as well as to
authors of new subscription types. New subscription types may be added here, but there is no
requirement to do so.

* [WebSocketSubscription2021](https://solid.github.io/notifications/websocket-subscription-2021) Editor's Draft.
* [StreamingHTTPSubscription2021](https://solid.github.io/notifications/streaming-http-subscription-2021) Editor's Draft.
* [EventSourceSubscription2021](https://solid.github.io/notifications/eventsource-subscription-2021) Editor's Draft.
* [LinkedDataNotificationsSubscription2021](https://solid.github.io/notifications/linkeddatanotifications-subscription-2021) Editor's Draft.
* [WebHookSubscription2021](https://github.com/solid/notifications/blob/main/webhook-subscription.md) Editor's Draft.

## Diagrams

### Mermaid

Sequence diagrams use [Mermaid](https://mermaid-js.github.io/mermaid/) as source syntax.
Github workflow generates them automatically before deplying github pages.
Any file ending with `.mmd` will be rendered into file ending with `.mmd.svg`.
For exmple `source-flow.mmd` results in `source-flow.mmd.svg` which can be referenced from the spec.

To preview diagrams locally following tools are available:

* [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/)
* [Docker image](https://hub.docker.com/r/matthewfeickert/mermaid-cli)
* [mermaid-cli](https://www.npmjs.com/package/@mermaid-js/mermaid-cli)
* [code editor extensions](https://github.com/mermaid-js/mermaid/blob/develop/docs/integrations.md#editor-plugins)

All `*.mmd.svg` files are ignored by git so they won't be commited to the repository.
