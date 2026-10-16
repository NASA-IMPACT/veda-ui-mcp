# The View travels twice: structuredContent and a JSON text block

A tool result carries its View as `structuredContent`, the channel the MCP Apps spec defines for widget data, and again as a JSON text content block. In mid-2026 Claude Desktop stripped `structuredContent` before handing the result to the widget sandbox while passing text blocks through, so a view read only from `structuredContent` rendered nothing there. `recoverView` tries `structuredContent` first and falls back to parsing text blocks, so the widget works in either kind of host.

## Consequences

- The model sees the View JSON in its transcript as well as the human summary: a few KB per collection search.
- The View Contract owns both halves (`encodeViewResult`, `recoverView`), and the stripped-host path is unit-tested in `view-contract.test.ts`.

## Considered

- `outputSchema` on the tools as the fix, on the theory that hosts forward `structuredContent` only for tools that declare one. It did not change what the widget received. The tools keep `outputSchema` anyway: it describes the result shape to the host and the model.
- Widget fetches its own data via `callServerTool` after mount. Rejected: runs every tool twice, and whether that path keeps `structuredContent` in the stripping host was unverified.

## Revisit when

- Every target host delivers `structuredContent` intact to the widget. Claude Desktop did when re-tested on 2026-10-16; claude.ai and ChatGPT were not yet checked. Check with the debug disclosure the view shell renders under a failing widget.
