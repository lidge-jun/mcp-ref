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

| Server | Description | Source |
|---|---|---|
| **Context7** | Library/framework docs lookup. Prevents API hallucination by fetching current docs. | [upstash/context7](https://github.com/upstash/context7) |
| **Playwright** | Browser automation via accessibility tree (not screenshots). Cross-browser testing. | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) |
| **Figma** | Bidirectional design-to-code. Read components/layout, write back to canvas. | [anthropics/mcp-server-figma](https://github.com/anthropics/mcp-server-figma) |
| **Sentry** | Error tracking. Pull stack traces, error events. Paste issue ID -> agent writes fix. | [getsentry/sentry-mcp](https://github.com/getsentry/sentry-mcp) |
| **Supabase** | Database schema, SQL execution, TypeScript type gen. Official Claude connector. | [supabase/mcp-server](https://github.com/supabase/mcp-server) |
| **Cloudflare** | 2,500+ endpoints with 99.9% token reduction via Code Mode. Workers/KV/D1/R2. | [cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare) |
| **Stripe** | Stripe API docs search + payment integration helpers. OAuth + restricted keys. | [stripe/agent-toolkit](https://github.com/stripe/agent-toolkit) |
| **Linear** | Issue tracking. Manage issues/sprints/projects from agent. Saves context-switching. | [anthropics/mcp-server-linear](https://github.com/anthropics/mcp-server-linear) |

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

## Sources

Research and curation based on:
- [MCP Token Cost Benchmark — 35x More Tokens Than CLI](https://onlycli.github.io/OnlyCLI/blog/mcp-token-cost-benchmark/)
- [Is MCP Dead? (Charles Chen, 2026-03)](https://chrlschn.dev/blog/2026/03/mcp-is-dead-long-live-mcp/)
- [Claude Code Skills vs MCP vs Plugins](https://www.morphllm.com/claude-code-skills-mcp-plugins)
- [15 MCP Servers Worth Installing (Codersera, 2026)](https://codersera.com/blog/best-mcp-servers-claude-code-cursor-2026/)
- [Cloudflare Code Mode MCP — 99.9% Token Reduction (InfoQ)](https://www.infoq.com/news/2026/04/cloudflare-code-mode-mcp-server/)
- [Supabase Official Claude Connector (2026-02)](https://supabase.com/blog/supabase-is-now-an-official-claude-connector)
- [ACP vs MCP Protocol Comparison (Context Studios)](https://www.contextstudios.ai/blog/acp-vs-mcp-the-protocol-war-that-will-define-ai-coding-in-2026)
- [MCP 2.0 Spec Changes (Claude Marketplace)](https://www.claudemarketplace.net/digest/mcp-2-everything-developers-need-to-know)
- Harness source analysis: [oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent), [oh-my-codex](https://github.com/code-yeongyu/oh-my-codex) (2026-05-29 corpus)
