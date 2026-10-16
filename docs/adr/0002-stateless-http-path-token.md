# Stateless HTTP transport, gated by a secret path segment

The Streamable HTTP endpoint builds a fresh `McpServer` and transport for every request and issues no session ids. Tool calls finish in seconds and need nothing from earlier calls, so any instance can serve any request: it runs as a single serverless function or as one long-lived process with the same code (`server/app.ts`). Non-POST methods on the endpoint return 405, since the SDK would otherwise hold a GET open as an SSE stream until the function times out.

Access is gated by `MCP_PATH_TOKEN`: when set, the endpoint moves to `/mcp/<token>` and bare `/mcp` returns 404. claude.ai custom connectors take a URL and nothing else, so the URL is the credential.

## Consequences

- Anyone holding the URL can call the tools. That is acceptable because the server only reads a public catalog; rotating the token cuts everyone off at once.
- No server-initiated notifications or resumable streams: those need sessions.
- The token lives in the host's environment, never in the repository, and never in logs: startup output prints the endpoint as `/mcp/<token>`. Anyone with access to the host's environment holds the credential; rotate it when that group changes.

## Considered

- OAuth for the connector. Not pursued: every person would pay a sign-in flow to try a read-only public-data tool.
- Per-IP rate limiting. Rejected: claude.ai connectors call from shared provider egress IPs, so a per-IP limit throttles every legitimate user together.

## Revisit when

- The server gains a tool that writes, costs money per call, or reads non-public data.
- The URL is shared beyond a known group (a connectors directory listing, a public post), so leaking it stops being a contained event.
