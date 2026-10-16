# veda-ui-mcp

MCP App: tools over the VEDA STAC catalog whose results render veda-ui-blocks widgets inline in chat hosts (Claude, ChatGPT). Node 22, npm, TypeScript, React 19, Vite single-file bundles.

## Read before changing code

- [CODING_STANDARDS.md](CODING_STANDARDS.md): how code here is written. Reviews check changes against it.
- [GLOSSARY.md](GLOSSARY.md): domain vocabulary (Collection, View, Widget, Pick, ...). Name things with its terms and respect its _Avoid_ lists.
- [docs/adr/](docs/adr/): binding decisions. A change that contradicts one says so explicitly.
- [ARCHITECTURE.md](ARCHITECTURE.md): MCP primer and repo map, for orientation.

## Done means

A change is done when `npm test` and `npm run build` (which typechecks first) pass, and, for anything touching a view, a tool's result shape, CSP or the blocks dependency, the happy path renders in a real MCP Apps host: `basic-host` from the [ext-apps repo](https://github.com/modelcontextprotocol/ext-apps) (`examples/basic-host`) against `npm run serve` on `:3001`. Check `run_demo` map render, picker cards, card click arriving as a user message, compare render, and that tiles and thumbnails load (a missing CSP origin shows up only here).

Install with plain `npm install`: its `postinstall` applies `patches/`. With `--ignore-scripts` (as CI uses) run `npx patch-package` yourself, or categorical colormaps fail with `[object Object]` 400s from the raster API.

## Deploying

Setup and env vars: [docs/deployment.md](docs/deployment.md). Production is a Vercel project deployed by hand from a checkout: `npx vercel deploy --prod` (no Git integration, so merging deploys nothing). `npx vercel link` writes `.env.local` with a short-lived OIDC token and appends to `.gitignore`: delete the first, revert the second. `MCP_PATH_TOKEN` lives only in the host's environment and local `.env` (gitignored).

## Issue tracker

GitHub issues in `NASA-IMPACT/veda-ui-mcp` via the `gh` CLI. This is a public partner repository: issue bodies, commits and PRs carry nothing internal (private repo links, tokens, internal hostnames).
