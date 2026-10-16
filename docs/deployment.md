# Deployment

HTTP-first: `POST /mcp` (Streamable HTTP, stateless: a fresh server per
request, no session) + `GET /health`. Other methods on `/mcp` return 405 (no
standalone SSE stream). The Express app is built in `server/app.ts`; it runs
as a serverless function or a long-lived process.

Env vars (all optional):

- `MCP_PATH_TOKEN` — shared-secret path segment: endpoint moves to
  `/mcp/<token>`, bare `/mcp` 404s. Letters/digits/`-`/`_` only. Required
  for any public deploy: it is the only access gate.
- `VEDA_STAC_ROOT` / `VEDA_RASTER_ROOT` — catalog/titiler roots.

Locally, `npm run serve` and `npm run start:prod` load these from `.env`
when it exists (gitignored).

The connector URL is `https://<domain>/mcp/<token>`. Verify a deploy with
`curl https://<domain>/health` → `{"status":"ok","transport":"http"}`.

## Vercel

`api/index.js` exports the Express app as one function; `vercel.json`
rewrites every path to it, runs `npm run build`, and ships `dist/*.html`
(the view bundles read at request time) via `includeFiles`.
`engines.node: 22.x` pins the runtime.

One-time setup: create a Vercel project, set `MCP_PATH_TOKEN`
(`openssl rand -hex 16`) for Production, make sure Deployment Protection
does not cover Production. Then deploy through Vercel's Git integration or
from a checkout:

```bash
npx vercel link            # once: pick the project
npx vercel deploy          # preview (behind Vercel's default protection)
npx vercel deploy --prod   # production
```

GitHub Actions only typechecks, tests and builds; it does not deploy.

## Heroku

`Procfile` runs `npm run start:prod`.

```bash
heroku create <app-name>
heroku config:set MCP_PATH_TOKEN=$(openssl rand -hex 16)
git push heroku main
```

Use a Basic dyno: Eco dynos sleep, and waking one takes ~10-30 s.

## Troubleshooting

- **500 / function crash on Vercel** — check the function logs; config
  validation errors print the reason. On Heroku, `heroku logs --tail`: if
  the `VEDA UI MCP (HTTP transport)` banner is absent, boot failed.
- **404 on `/mcp`** — expected when `MCP_PATH_TOKEN` is set; use
  `/mcp/<token>`.
- **First request slow** — cold start (Vercel function or Heroku Eco dyno).
- **HTTPS required** — Claude and ChatGPT only connect to `https://`
  endpoints.
- **Tool answers "cannot be mapped" / "unknown collection"** — corrective by
  design; the model retries with a valid collection.
