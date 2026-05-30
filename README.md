# mcp-ref

Curated MCP server registry for cli-jaw. **Only genuinely useful servers** — no redundant wrappers.

## Philosophy

Most MCP servers are useless in 2026. CLI tools and skills replaced them:
- Filesystem MCP? CLI has sandboxed file access.
- Git MCP? `git` CLI is built in.
- Web search MCP? WebSearch is built in.
- Memory MCP? CLAUDE.md / memory files.
- Sequential thinking? Extended thinking is built in.

This registry only includes servers that provide access to **things CLI tools cannot replicate**.

## Current Servers (8)

| Server | Why it's useful |
|---|---|
| **Context7** | Library docs lookup — prevents API hallucination |
| **Playwright** | Browser automation via accessibility tree, not screenshots |
| **Figma** | Bidirectional design-to-code |
| **Sentry** | Error tracking — stack traces directly in agent |
| **Supabase** | Database schema + SQL + type generation |
| **Cloudflare** | 2,500+ endpoints with 99.9% token reduction (Code Mode) |
| **Stripe** | Payment API docs search + integration helpers |
| **Linear** | Issue tracking without context-switching |

## Token Budget

Each MCP server adds ~500-1,000 tokens to context. Recommended max: **3-4 servers**.

Claude Code's ToolSearch mitigates this (95% reduction), but more servers = more noise.

## Not Included (and why)

| Server | Why skipped |
|---|---|
| Filesystem | CLI built-in |
| Git/GitHub | `git`/`gh` CLI built-in |
| Web search (Brave, Exa) | WebSearch built-in |
| Fetch | WebFetch built-in |
| Memory | CLAUDE.md / memory files |
| Sequential Thinking | Extended thinking built-in |
| Community wrappers | 36.7% have SSRF vulnerabilities |

## MCP vs ACP

- **MCP** = agent-to-tool (query database, browse web)
- **ACP/A2A** = agent-to-agent (delegate to another agent)

ACP merged into Google A2A under Linux Foundation. For solo/small teams, A2A is irrelevant.

## Harness Built-in Reference

These are NOT standalone MCP servers — they're built into omo/omx harnesses. Listed for architecture reference.

### omo (oh-my-openagent) — 5 built-in tools

| Tool | Type | What it does |
|---|---|---|
| **grep** | Built-in | ripgrep binary auto-downloaded per OS. Structural search. |
| **ast-grep** | MCP (stdio) | AST-level search/replace via `@ast-grep/napi`. Metavariable patterns. |
| **LSP** | MCP (stdio) | Language server protocol — diagnostics, symbols, hover, references. |
| **grep.app** | MCP (remote) | Global public code search across GitHub. `https://mcp.grep.app` |
| **context7** | MCP (remote) | Same as standalone context7. |

omo's 3-tier MCP: built-in → Claude Code `.mcp.json` → skill-embedded YAML

### omx (oh-my-codex) — 6 built-in MCP servers

| Server | What it does |
|---|---|
| **omx_code_intel** | 9 tools: tsc diagnostics, symbols, hover, grep references, ast-grep search/replace |
| **omx_state** | Mode/workflow state management |
| **omx_memory** | Cross-session project memory |
| **omx_trace** | Agent flow timeline/stats |
| **omx_wiki** | Persistent project knowledge base |
| **omx_hermes** | Safe dispatch/status/artifacts coordination |

Key insight: `omx_code_intel`'s `lsp_find_references` internally calls `grep -rn -w`. It wraps grep in structured interfaces, not replaces it.

### Standalone from harness built-ins

`grep.app` (omo default remote) can be installed standalone:
```json
{ "url": "https://mcp.grep.app/mcp" }
```

## Adding a Server

Only add servers that provide access to **external services CLI tools can't reach**. Submit a PR with:
- `name`, `description` (one line explaining WHY it's useful, not just what it does)
- `category`, `type`, `config`, `tags`, `url`
