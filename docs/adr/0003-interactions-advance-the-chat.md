# Interactions advance the chat

Each tool renders one step View (picker, map, compare), and a user action inside a View is sent to the conversation as a user message (`app.sendMessage`): clicking a collection card posts "I picked the dataset ... (id: ...)", and the model decides the next tool call. Views have no toolbar and no widget-local navigation.

## Consequences

- The model stays the orchestrator, so the flow works the same in any host and the transcript records every step.
- Bundles split along dependencies, not Views: the Picker is its own bundle and ships without the map stack; the Map View and Compare View share `map.html`, which dispatches on `kind`. Each bundle is a resource with a CSP scoped to what it loads.
- If a host rejects `sendMessage`, the picker shows an error telling the user to type the pick instead.

## Considered

- One app-like widget with a toolbar that switched views by calling tools itself (`callServerTool`). Built first and replaced in July 2026: navigation inside the widget happens out of the model's sight, so the model cannot drive or explain the next step, and one bundle carried every view's dependencies.

## Revisit when

- A flow needs many small adjustments to one View (a time slider, panning between dates) where a model round-trip per change is too slow; `app.updateModelContext` can keep the model informed without a message per change.
