# AI Competitor Intelligence Agent

An AI agent for competitive intelligence — cross-cutting audits combining SEO, ads, social, and review data into one competitor-vs-client comparison — backed by real data APIs.

Part of [Agency Agents OS](https://github.com/Anil-matcha/agency-agents-os), an open ecosystem of specialized AI agents for real business work.

## Related Projects

- [Agency Agents OS](https://github.com/Anil-matcha/agency-agents-os) — the central catalog this repo is part of.
- [ai-seo-agent](https://github.com/SamurAIGPT/ai-seo-agent) — supplies the live domain/backlink data this repo's competitive-audit sub-agent cross-cuts.
- [ai-ads-agent](https://github.com/SamurAIGPT/ai-ads-agent) — shares this repo's ad-library-mining capability.
- [ai-social-agent](https://github.com/SamurAIGPT/ai-social-agent) — feeds the social-presence side of a competitive audit.
- [ai-reputation-agent](https://github.com/SamurAIGPT/ai-reputation-agent) — feeds the review/sentiment side of a competitive audit.
- [MuAPI MCP docs](https://muapi.ai/docs/mcp) — connect this repo's `SKILL.md` files via MCP.
- [MuAPI access keys](https://muapi.ai/access-keys) — create the API key this agent needs.

## What this covers

This repo is the umbrella for anything an agency or in-house team would call "the competitor intelligence agent": pulling a competitor's SEO position, ad activity, social presence, and review/reputation signals into a single comparison report against the client, rather than checking each channel by hand. It deliberately cross-cuts other umbrellas — `ai-seo-agent`, `ai-ads-agent`, `ai-social-agent`, `ai-reputation-agent` — pulling one signal from each into a combined view instead of duplicating any of their individual capabilities.

## Sub-agents

| Agent | Does | Status |
|---|---|---|
| [Competitive Audit](agents/competitive-audit/SKILL.md) | Cross-cutting audit of a competitor combining SEO position, social presence, and review/reputation signals into one comparison report against the client | Coming Soon |
| [Ad Library Mining](agents/ad-library-mining/SKILL.md) | Analyzes which ads a competitor is actively running — creative, messaging, and longevity as a signal of what's working | Blueprint |

## Required Muapi APIs

- `seo.search_performance` — competitor and client organic search position/ranking signals (see [`ai-seo-agent`](https://github.com/SamurAIGPT/ai-seo-agent)).
- `ads.ad_library_search` — public ad-library-style lookup of a competitor's actively running ads (see [`ai-ads-agent`](https://github.com/SamurAIGPT/ai-ads-agent)).
- `social.read_posts` — competitor and client social account activity and engagement (see [`ai-social-agent`](https://github.com/SamurAIGPT/ai-social-agent)).
- `reputation.review_search` — competitor and client review/reputation signals across public review sources (see [`ai-reputation-agent`](https://github.com/SamurAIGPT/ai-reputation-agent)).

These capabilities are shared with the sibling umbrella repos above; this repo's sub-agents combine them into cross-channel comparison reports rather than owning any one channel.

See each sub-agent's `SKILL.md` for the specific capabilities it uses.

## Setup

1. Create a Muapi account and API key at [muapi.ai](https://muapi.ai).
2. Review the [Muapi API quickstart](https://muapi.ai) and [OpenAPI schema](https://api.muapi.ai/openapi.json) for the relevant endpoints.
3. Load the `SKILL.md` for the sub-agent you need into your agent runtime (hosted agent, MCP client, or custom LLM app), or follow it manually.


## Using with an AI agent

Every sub-agent's `SKILL.md` is model- and runtime-agnostic — it's plain Markdown, so it works with any LLM agent, not just Claude. Two integration paths:

**As an MCP connection (the agent gets live Muapi tools):**

Muapi runs an MCP server at `https://api.muapi.ai/mcp` that any MCP-compatible client can connect to — Cursor, Windsurf, Claude, or your own custom agent.

- **Cursor / Windsurf / other clients with a header field:** connect to `https://api.muapi.ai/mcp` with an `Authorization: Bearer YOUR_MUAPI_KEY` header.
- **claude.ai / Claude Cowork / other connector UIs with no header field:** use the URL-embedded key form instead, `https://api.muapi.ai/mcp/YOUR_MUAPI_KEY`, via Settings → Connectors → Add custom connector.
- **Claude Code / Claude Desktop:** `claude mcp add muapi -e MUAPI_API_KEY=YOUR_MUAPI_KEY -- muapi mcp serve` (uses the muapi CLI's stdio transport — Claude Code's HTTP MCP client doesn't reliably inject tools).

Full setup details for every client: [muapi.ai/docs/mcp](https://muapi.ai/docs/mcp).

**As agent instructions (any LLM follows the workflow directly):**

Drop a sub-agent's `SKILL.md` into a Claude Code project's `.claude/skills/` directory, paste it into a custom-GPT/Project's system instructions, hand it to an autonomous agent framework as a tool spec, or attach it directly in a chat conversation — then ask the agent to follow it.

## Read-only vs. write actions

Both sub-agents are `read-only` — they gather and compare public signals into a report. Neither publishes, files a complaint, responds to a review, or takes any action on a competitor's or client's own accounts.

## Status and limitations

**Ad Library Mining is Blueprint**: it only needs `ads.ad_library_search`, which is coded but not yet live on Muapi's production API — see its own `SKILL.md` for exact scope (currently-visible listings only, no spend/impression data). **Competitive Audit remains Coming Soon**: it needs all four of `seo.search_performance` (already live), `ads.ad_library_search` (coded, not live), `social.read_posts` (coded, not live), and `reputation.review_search` (not yet built) — the last of these, review/reputation data, is the one still-unstarted piece blocking this sub-agent specifically.

## Contributing

See [Agency Agents OS CONTRIBUTING.md](https://github.com/Anil-matcha/agency-agents-os/blob/main/CONTRIBUTING.md).

## License

[MIT](LICENSE)
