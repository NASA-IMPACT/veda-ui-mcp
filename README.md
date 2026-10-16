# veda-ui-mcp

Ask an AI assistant about Earth data and get VEDA's collection picker,
raster map and swipe-compare map inline in the chat. An MCP App over the
VEDA STAC catalog.

## Try it

Paste this into a coding agent (Claude Code, Cursor, or similar):

```text
Read https://github.com/NASA-IMPACT/veda-ui-mcp/blob/main/README.md and help me try the VEDA UI MCP app. First ask whether I want to connect to the hosted instance (I need its connector URL from the VEDA team) or run it locally with Claude Desktop. Then walk me through the steps for that option one at a time, run the commands for me where you can, and confirm each step worked before the next.
```

**Hosted instance.** Get the connector URL (`https://<domain>/mcp/<token>`)
from the VEDA team and keep it private. In claude.ai or Claude Desktop:
Settings → Connectors → Add custom connector, paste the URL, no
authentication. In Claude Code:
`claude mcp add --transport http veda <connector URL>`.

**Locally.** Needs Node 22 and Claude Desktop, no token:

```bash
git clone https://github.com/NASA-IMPACT/veda-ui-mcp
cd veda-ui-mcp
npm install
npm run setup:claude   # registers with Claude Desktop and Claude Code
```

Quit and reopen Claude Desktop. To unregister: `npm run setup:claude -- --remove`.

Then ask: "Run a demo of VEDA UI MCP", "What NO2 data does VEDA have?",
"Compare NO2 in 2020 and 2021", "Show land cover for Bangladesh".

## Develop

```bash
npm start   # build + serve over HTTP at http://localhost:3001/mcp
npm run dev # watch UI bundles + restart server on change
npm test
```

Check widgets end to end in `basic-host`
([ext-apps repo](https://github.com/modelcontextprotocol/ext-apps),
`examples/basic-host`) against `http://localhost:3001/mcp`.

- [ARCHITECTURE.md](ARCHITECTURE.md) — MCP primer and repo map
- [GLOSSARY.md](GLOSSARY.md) — domain vocabulary
- [docs/deployment.md](docs/deployment.md) — deploying, env vars, troubleshooting
