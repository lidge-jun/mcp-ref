<p align="center">
  <strong>mcp-ref</strong><br>
  Curated MCP server registry for cli-jaw.
</p>

<p align="center">
  <a href="https://github.com/lidge-jun/mcp-ref/actions/workflows/ci.yml"><img src="https://github.com/lidge-jun/mcp-ref/actions/workflows/ci.yml/badge.svg" alt="CI"></a>
  <a href="https://github.com/lidge-jun/mcp-ref/actions/workflows/pages.yml"><img src="https://github.com/lidge-jun/mcp-ref/actions/workflows/pages.yml/badge.svg" alt="Pages"></a>
  <img src="https://img.shields.io/badge/registry-v3-2563eb" alt="Registry version 3">
  <img src="https://img.shields.io/badge/servers-13-111827" alt="13 curated servers">
</p>

---

# mcp-ref

`mcp-ref` is a small, opinionated MCP registry for `cli-jaw`. It keeps only
servers that give agents access to surfaces ordinary CLI tools cannot replace:
current library docs, real browsers, design canvases, observability systems,
databases, payment platforms, issue trackers, cloud APIs, semantic search, and
language-server intelligence.

The registry is intentionally not a comprehensive catalog. It is a curated
allowlist for low-noise agent setups.

## Public Surface

| Surface | Status |
|---------|--------|
| Registry | `/registry.json`, version 3, 13 server entries |
| README | Philosophy, server table, install policy, verification policy |
| GitHub Pages | `/docs/index.html` static landing page, ready for Pages deployment |
| CI | JSON validation, count drift checks, README/docs link checks |
| License | No root license file is currently declared |

Remote Pages state: GitHub Pages is not enabled for this repository yet
(`GET /repos/lidge-jun/mcp-ref/pages` returns 404). The added workflow deploys
`/docs` after an authorized push.

## Why This Exists

Every MCP server adds prompt, permission, and maintenance surface. Many servers
duplicate what a coding CLI already has: filesystem access, `git`, `gh`, web
fetching, memory files, or extended reasoning.

`mcp-ref` filters for a narrower rule:

> Include an MCP server only when it unlocks an external capability the local
> CLI cannot faithfully reproduce.

That keeps the agent context smaller and the tool list easier to reason about.

## Registry Snapshot

| Server | Category | Why it stays |
|--------|----------|--------------|
| Context7 | development | Current library/framework documentation lookup |
| Playwright | testing | Browser automation through accessibility and DOM state |
| Figma | design | Bidirectional design canvas access |
| Sentry | development | Production error traces and frequency data |
| Supabase | database | Schema exploration, SQL, and type generation |
| Cloudflare (Code Mode) | cloud | Large Cloudflare API surface with compact tool access |
| Stripe | development | Payments API docs and helper workflows |
| Linear | productivity | Issue and project context from the tracker |
| grep.app | search | Global GitHub code search |
| ast-grep | development | AST-aware structural search and rewrite |
| LSP Tools | development | Diagnostics, symbols, hover, references |
| Tavily Web Search | search | Structured web extraction for agent workflows |
| Exa Search | search | Semantic web search and related-page discovery |

## Quickstart

Clone the registry:

```bash
git clone https://github.com/lidge-jun/mcp-ref.git
cd mcp-ref
```

Inspect the catalog:

```bash
jq '.version, (.servers | keys)' registry.json
jq -r '.servers | to_entries[] | [.key, .value.category, .value.type] | @tsv' registry.json
```

Use one entry in your global MCP configuration. Do not copy every server by
default. Start with one server, verify it is actually used, then add another.

Example: Context7 entry from the registry:

```bash
jq '.servers.context7.config' registry.json
```

## Registry Schema

Each server entry is keyed by a stable lowercase id:

```json
{
  "name": "Context7",
  "description": "Library/framework docs lookup...",
  "category": "development",
  "type": "local",
  "config": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp"]
  },
  "tags": ["docs", "library"],
  "url": "https://github.com/upstash/context7"
}
```

Required fields:

| Field | Purpose |
|-------|---------|
| `name` | Human-readable server name |
| `description` | One-line reason the server is useful |
| `category` | Broad routing group |
| `type` | `local` or `remote` |
| `config` | Launch or connection hints |
| `tags` | Short discovery labels |
| `url` | Source or service URL |

## Selection Policy

Include a server when it meets at least one of these criteria:

- It reaches an authenticated external system.
- It exposes live state a CLI cannot infer.
- It reduces a high-volume API surface to a compact workflow.
- It provides structural code/design intelligence, not just text search.
- It has a credible upstream and a clear maintenance path.

Do not include wrappers for:

| Server class | Reason |
|--------------|--------|
| Filesystem | Coding CLIs already have sandboxed file access |
| Git | `git` is already available |
| GitHub basics | `gh` is already available |
| Fetch-only servers | Web fetch/search tools already cover this |
| Memory-only servers | Project memory files are simpler and inspectable |
| Reasoning helpers | Modern models already support extended reasoning |
| Thin community wrappers | Higher supply-chain and SSRF risk for little gain |

## Verification Policy

Local checks:

```bash
jq empty registry.json
jq -e '.version == 3 and (.servers | length == 13)' registry.json
git diff --check
```

Static documentation checks should confirm:

- README server count matches `registry.json`.
- `/docs/index.html` has canonical, Open Graph, and Twitter metadata.
- Local assets referenced by docs exist.
- Workflow files validate JSON and documentation drift before deploy.

Remote CI signal: this repository currently has no completed Actions history.
The added `CI` and `Pages` workflows will run after an authorized push.

## Security Notes

- Tokens in `registry.json` are placeholders such as `YOUR_TOKEN`; do not commit
  real credentials.
- Prefer global user-level MCP configuration over project-local `.mcp.json`.
- Treat every MCP server as a new trust boundary: inspect upstream source,
  required scopes, and network behavior before enabling it.
- Keep the active MCP set small. Most projects should run no more than three or
  four MCP servers at once.

## Adding a Server

1. Confirm the server provides a capability the CLI cannot already perform.
2. Add one entry under `servers` in `registry.json`.
3. Keep the `description` focused on why the server is useful.
4. Run the local verification commands above.
5. Update README and Pages copy if the count, categories, or positioning change.

## Sources

The current curation was informed by MCP token-cost discussions, Cloudflare Code
Mode coverage, Supabase and Context7 connector patterns, and source analysis of
the `oh-my-openagent` / `oh-my-codex` harnesses. Treat this repo as a living
curation layer, not as a neutral directory.
