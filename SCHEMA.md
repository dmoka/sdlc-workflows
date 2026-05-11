# Workflow frontmatter schema

Every `.mdx` file under `workflows/` has YAML frontmatter followed by an optional MDX body. This document is the authoritative reference for what fields are valid.

## Required fields

| Field | Type | Notes |
|---|---|---|
| `title` | string | Title-case, e.g. `OpenAI Agents SDK — Handoffs` |
| `tagline` | string | One sentence, ≤180 chars, no trailing period preferred |
| `tier` | enum | One of: `universal`, `multi-file`, `tool-specific`, `concept` |
| `canonical_url` | URL | The official source for this workflow (vendor docs, blog, repo) |

## Optional fields

| Field | Type | Notes |
|---|---|---|
| `attribution` | string \| null | Originator (person, vendor, or org). Defaults to null. |
| `tags` | string[] | 1–5 descriptive labels: `loop`, `multi-agent`, `claude-code`, etc. |
| `setup_command` | string (block) | The install + run incantation. Use `\|` for multi-line YAML. |
| `when_to_use` | string | One sentence: when this workflow is the right choice |
| `when_not_to_use` | string | One sentence: when to avoid it |
| `upstream` | object | GitHub repo info for license-gated mirroring (see below) |
| `sources` | source[] | References (see schema below) |
| `related` | related | Cross-links into the site's knowledge base (see schema below) |
| `draft` | boolean | Default `false`. Set `true` to hide from the site without deleting. |

## `upstream` shape

Optional. When present, the build pipeline detects the upstream license and decides whether to mirror the named files or just reference-link them.

```yaml
upstream:
  repo: owner/repo                     # e.g. "openai/openai-agents-python"
  ref: main                            # branch, tag, or commit SHA. Default "main".
  paths:                               # files to mirror (empty array = reference-only)
    - README.md
    - templates/commands/specify.md
```

**License gate** (applied at build time, no contributor decision needed):

| Upstream LICENSE detected | Behavior |
|---|---|
| MIT / Apache-2.0 / BSD / MPL-2.0 / CC0 | **Mirror** — full content embedded in card + payload |
| AGPL / GPL / Proprietary / no LICENSE | **Reference** — list URLs, do not embed |

## `sources` shape

```yaml
sources:
  - title: Page or post title         # optional
    author: Author Name               # optional
    url: https://...                  # required
    year: 2025                        # optional
```

At least one source is encouraged. Sourcing is the cornerstone of the catalog's credibility.

## `related` shape

Slug references into the site's knowledge base. The slug is the entry's filename (without `.mdx`) in the corresponding directory of the main site.

```yaml
related:
  laws: []                            # slugs of laws
  patterns: []                        # slugs of patterns
  antiPatterns: []                    # slugs of anti-patterns
  practices: []                       # slugs of best practices
  glossary: []                        # slugs of glossary terms
  tools: []                           # slugs of tools
  aiAssistants: []                    # slugs of AI assistants
  workflows: []                       # slugs of sibling workflows
```

**Note:** the slugs must exist on the main site. If you're unsure what slugs are available, check the live site at [agentic-sdlc.dev](https://agentic-sdlc.dev) — each detail page URL ends with its slug.

Don't worry about being exhaustive. 3–6 strong links across 2–3 sections is better than 30 weak ones. The validator caps at 3 per section; extras are dropped.

## `tier` semantics

| Tier | Means | Typical example |
|---|---|---|
| `universal` | Paste into any LLM, works as-is. No setup needed. | Reflexion loop, Plan-approval gate |
| `multi-file` | Needs files on disk (PROMPT.md, AGENTS.md, etc.) but no specific tool | Ralph Wiggum loop, PIV Loop |
| `tool-specific` | Requires a specific CLI / IDE / SDK installed | Cline Plan & Act, OpenAI Handoffs, Cursor BG agents |
| `concept` | Architectural idea, not a runnable recipe | Orchestrator-Workers, Venutian Antfarm |

When in doubt, pick `tool-specific` — it's the most common.

## MDX body

**Reference-mode and Mirror-mode cards:** leave the body empty. The site renderer assembles the card from frontmatter + (optional) mirrored upstream artifacts.

**Editorial-mode cards:** write ~200 words of prose. See [CONTRIBUTING.md](./CONTRIBUTING.md#editorial-body--voice-rules) for the voice rules.

## Slug rules

- Filename: `kebab-case-slug.mdx`
- Allowed characters: `[a-z0-9-]+`
- Maximum length: 60 characters
- Must be unique across `workflows/`
- Should NOT collide with slugs already used on the main site for `patterns/` or `glossary/`. If a pattern by the same name exists on the site, suffix your workflow with `-loop`, `-plugin`, `-cycle`, etc. (e.g., `evaluator-optimizer-loop` because `evaluator-optimizer` is a pattern slug).

## Full example: minimum-viable Reference card

```yaml
---
title: My Vendor — My Workflow
tagline: One-sentence pitch describing what the workflow does.
attribution: My Vendor
tier: tool-specific
canonical_url: https://my-vendor.com/docs/my-workflow
setup_command: |
  npm install -g my-vendor-cli
  my-vendor init
when_to_use: When you need X under condition Y.
when_not_to_use: When Z is true.
tags: [my-vendor, multi-agent]
sources:
  - title: My Vendor — official docs
    url: https://my-vendor.com/docs/my-workflow
    year: 2025
---
```

11 lines of frontmatter, no body. Fully valid.
