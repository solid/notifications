# Solid Notifications

This repository hosts the technical reports for Solid Notifications.

These reports are incubated by the [Solid Notifications Panel](https://github.com/solid/notifications-panel) for inclusion in the [Solid Technical Reports](https://solidproject.org/TR/). The reports are within the scope of the [Notifications Panel Charter](https://github.com/solid/process/blob/main/notifications-panel-charter.md).

## Technical Reports
* [Solid Notifications Protocol](https://solid.github.io/notifications/protocol) Editor's Draft.

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
