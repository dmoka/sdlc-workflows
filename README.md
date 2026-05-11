# sdlc-workflows

The open-source workflow catalog behind **[Agentic Software Development](https://agentic-sdlc.dev)**.

Each `.mdx` file under [`workflows/`](./workflows) is a copy-pasteable orchestration recipe for AI coding agents — Ralph, Spec Kit, PIV Loop, AGENTS.md, Antfarm, and 30+ others. The site at [agentic-sdlc.dev](https://agentic-sdlc.dev) renders these into card-grid + detail pages with an "Open in ChatGPT / Claude" menu.

## Quick links

- 🌐 **Site:** [agentic-sdlc.dev](https://agentic-sdlc.dev)
- 📝 **Contribute a workflow:** [CONTRIBUTING.md](./CONTRIBUTING.md)
- 📋 **Frontmatter schema:** [SCHEMA.md](./SCHEMA.md)
- ⚖️ **License:** MIT — workflows are free to use, modify, and redistribute with attribution.

## What's a "workflow"?

A workflow is a reusable orchestration pattern for AI coding agents — a recipe you can copy into your LLM of choice. Categorized by **tier**:

- **`universal`** — a prompt or pattern that works in any LLM (ChatGPT, Claude, Gemini)
- **`multi-file`** — needs files on disk (e.g., `AGENTS.md`, `PROMPT.md`)
- **`tool-specific`** — requires a specific tool (Claude Code, Cursor, Aider, …)
- **`concept`** — an architectural idea, not a specific runnable recipe

## Example workflow

A typical reference-mode workflow MDX (pointing to vendor docs):

```yaml
---
title: OpenAI Agents SDK — Handoffs
tagline: Each agent exposes transfer_to_X tools; the next response is owned
  by whichever agent is handed control.
attribution: OpenAI
tier: tool-specific
canonical_url: https://openai.github.io/openai-agents-python/handoffs/
setup_command: |
  pip install openai-agents
when_to_use: Specialist should own the next turn, not just help in the background.
when_not_to_use: When you need a single observable trace through one agent.
tags: [openai, sdk, handoffs, multi-agent]
sources:
  - title: OpenAI Agents SDK — Handoffs
    url: https://openai.github.io/openai-agents-python/handoffs/
    year: 2025
related:
  patterns: [routing, orchestrator-workers]
  tools: [openai-agents-sdk]
---
```

For full field documentation see [SCHEMA.md](./SCHEMA.md). For contribution rules and the body templates for editorial-mode cards, see [CONTRIBUTING.md](./CONTRIBUTING.md).

## How a contribution becomes live

1. You PR a new `.mdx` under [`workflows/`](./workflows)
2. The frontmatter-validator Action runs on the PR — blocks merge on schema violations
3. Maintainer reviews and merges
4. A GitHub Action POSTs to the [agentic-sdlc.dev](https://agentic-sdlc.dev) deploy hook
5. The site rebuilds (~90s) — your workflow is live with a detail page, an LLMs ▾ menu, and cross-links into the knowledge base

End-to-end: PR merge → live on site in under 2 minutes.

## License

MIT — see [LICENSE](./LICENSE). Each individual workflow may reference upstream content under its own license; the site renderer respects upstream license terms automatically (mirror only if permissive, otherwise reference-link with attribution).
