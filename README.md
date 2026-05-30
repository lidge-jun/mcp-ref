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

## Adding a Server

Only add servers that provide access to **external services CLI tools can't reach**. Submit a PR with:
- `name`, `description` (one line explaining WHY it's useful, not just what it does)
- `category`, `type`, `config`, `tags`, `url`
