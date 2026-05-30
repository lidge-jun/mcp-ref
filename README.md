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

## Current Servers (11)

| Server | Description | Source | Origin |
|---|---|---|---|
| **Context7** | Library/framework docs lookup. Prevents API hallucination. | [upstash/context7](https://github.com/upstash/context7) | |
| **Playwright** | Browser automation via accessibility tree. Cross-browser testing. | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | |
| **Figma** | Bidirectional design-to-code. Read/write Figma canvas. | [anthropics/mcp-server-figma](https://github.com/anthropics/mcp-server-figma) | |
| **Sentry** | Error tracking. Stack traces -> agent writes fix. | [getsentry/sentry-mcp](https://github.com/getsentry/sentry-mcp) | |
| **Supabase** | Database schema + SQL + TypeScript type gen. | [supabase/mcp-server](https://github.com/supabase/mcp-server) | |
| **Cloudflare** | 2,500+ endpoints, 99.9% token reduction (Code Mode). | [cloudflare/mcp-server-cloudflare](https://github.com/cloudflare/mcp-server-cloudflare) | |
| **Stripe** | Stripe API docs search + payment helpers. | [stripe/agent-toolkit](https://github.com/stripe/agent-toolkit) | |
| **Linear** | Issue tracking from agent. Saves context-switching. | [anthropics/mcp-server-linear](https://github.com/anthropics/mcp-server-linear) | |
| **grep.app** | Global GitHub code search. Find real-world implementations. | [grep.app](https://grep.app) | omo built-in |
| **ast-grep** | AST structural code search/replace. Language-aware patterns. | [ast-grep](https://github.com/ast-grep/ast-grep) | omo built-in |
| **LSP Tools** | Code intelligence — diagnostics, symbols, hover, references. | [lsp-tools-mcp](https://github.com/code-yeongyu/oh-my-openagent/tree/main/packages/lsp-tools-mcp) | omo built-in |

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

## Harness Origins

Three servers (grep.app, ast-grep, LSP Tools) originated as omo (oh-my-openagent) built-ins but are **standalone installable** via this registry.

### omo (oh-my-openagent) architecture

omo uses a 3-tier MCP system: built-in → Claude Code `.mcp.json` → skill-embedded YAML.
Its built-in tools include ripgrep (auto-downloaded binary), ast-grep MCP, LSP MCP, grep.app (remote), and context7.

### omx (oh-my-codex) architecture

omx provides 6 built-in MCP servers (state, memory, code-intel, trace, wiki, hermes).
`omx_code_intel` wraps tsc/ast-grep/grep behind 9 structured tools — `lsp_find_references` internally calls `grep -rn -w`.

Key insight: these harnesses don't replace grep — they wrap it in structured interfaces (symbols, AST, diagnostics).

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
