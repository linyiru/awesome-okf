# Awesome OKF [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for the **Open Knowledge Format (OKF)** — Google's open, vendor-neutral spec for representing knowledge as Markdown files with YAML frontmatter that AI agents can read without custom integrations. This list practices what it catalogs: it is itself published as a conformant OKF bundle (see below).

OKF was announced by Google Cloud in June 2026; v0.2 followed in July 2026. It formalizes the "LLM-wiki" pattern into a portable, interoperable format: knowledge lives as plain Markdown files with structured frontmatter (`type`, `title`, `description`, `resource`, `tags`, plus optional provenance, trust, and lifecycle fields) and cross-links that form a knowledge graph. No SDK, no runtime, no lock-in — just files you can ship in a tarball, host in Git, or mount on a filesystem.

## Contents

- [OKF at a Glance](#okf-at-a-glance)
- [Specification](#specification)
- [Official Tools & Reference Implementations](#official-tools--reference-implementations)
- [Sample Bundles](#sample-bundles)
- [Community Tools](#community-tools)
- [Articles & Guides](#articles--guides)
- [Background & Origins](#background--origins)
- [Built on the LLM-Wiki Pattern](#built-on-the-llm-wiki-pattern)
- [Related Formats & Concepts](#related-formats--concepts)
- [Community](#community)

## OKF at a Glance

A bundle is a directory tree of Markdown files, and the directory structure is independent of the domain. The essentials from the v0.2 spec:

| Aspect                | Rule                                                                                                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Concept               | One Markdown file: YAML frontmatter plus a free-form Markdown body.                                                                                                                                                 |
| Required field        | `type` — a non-empty string identifying the kind of concept.                                                                                                                                                        |
| Recommended fields    | `title`, `description`, `resource` (a URI for the underlying asset), `tags`.                                                                                                                                        |
| Provenance & trust    | Optional `sources` (with per-source credibility signals), `generated` (`{ by, at }`; supersedes v0.1 `timestamp`), and `verified` — consumers derive trust tiers (unverified / machine-confirmed / human-reviewed). |
| Lifecycle             | Optional `status` (`draft` / `stable` / `deprecated`; absent means `stable`) and `stale_after` (an absolute date).                                                                                                  |
| Attested computations | A `type: Attested Computation` concept carries a sanctioned computation (`runtime`, `parameters`, `executor`, `attester`) so a consumer can confirm a value was produced the blessed way, not improvised.           |
| Actors                | `generated.by` / `verified[].by` use `<producer>/<version>` for agents, `human:<id>` for people, `process:<id>` for processes.                                                                                      |
| Reserved filenames    | `index.md` (directory listing for progressive disclosure) and `log.md` (update history); every other `.md` is a concept. A bundle-root `index.md` may declare `okf_version` in frontmatter.                         |
| Links                 | A link from concept A to B asserts a relationship; use bundle-relative paths (starting with `/`) or ordinary relative paths.                                                                                        |
| Conformance           | Every non-reserved `.md` file has parseable frontmatter with a non-empty `type`; reserved files follow their defined structure when present.                                                                        |
| Consumer tolerance    | Consumers must tolerate missing optional fields (including every trust and provenance family), unknown `type` values, broken links, and a missing `index.md`.                                                       |
| Extensions            | Producers may add custom keys; consumers must preserve unknown fields.                                                                                                                                              |

> This list is also published as a conformant OKF v0.2 bundle under [`bundle/`](bundle), generated from this README by [`scripts/build-okf-bundle.mjs`](scripts/build-okf-bundle.mjs).

## Specification

- [OKF v0.2 Specification (SPEC.md)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) - The universal, vendor-neutral spec, including conformance criteria and reserved filenames. v0.2 (July 2026) makes provenance (`sources`), trust (`generated` / `verified`), lifecycle (`status` / `stale_after`), and attested computations first-class; it supersedes v0.1's `timestamp` field and body `# Citations` list. The [v0.1 text stays readable at a pinned commit](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/ee67a5ca27044ebe7c38385f5b6cffc2305a9c1a/okf/SPEC.md).
- [GoogleCloudPlatform/knowledge-catalog `okf/`](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf) - The home repository for the format, reference code, and samples.
- [OKF README](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/README.md) - Overview of OKF philosophy, installation, and usage.

## Official Tools & Reference Implementations

- [Reference Agent](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/src/reference_agent) - Python implementation (built on Google's Agent Development Kit) that produces and visualizes OKF bundles: it drafts OKF documents from a BigQuery source, enriches them with web-crawled provenance (`sources` frontmatter with per-claim footnote attribution, since v0.2), and ships a `viewer/generator.py` that renders the bundle as a self-contained, interactive graph.
- [Knowledge Catalog Enrichment toolbox](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/toolbox/enrichment) - A ready-to-use agent and customizable harness (TypeScript) to produce, evolve, and maintain metadata in Knowledge Catalog. Includes a server that exposes a Markdown fileset as an MCP server.
- [Knowledge Catalog mdcode](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/toolbox/mdcode) - Manage metadata as source-code artifacts, with git-style pull/push sync between OKF Markdown and BigQuery / Dataplex / Knowledge Catalog.

## Sample Bundles

Four conformant, ready-to-browse bundles from the home repository, each with a self-contained interactive `viz.html` graph viewer. The first three are built from public BigQuery datasets:

- [GA4 e-commerce](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/ga4) - Google Analytics 4 e-commerce metadata.
- [Stack Overflow](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/stackoverflow) - Schema graph for the public Q&A dataset.
- [Bitcoin](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/crypto_bitcoin) - On-chain concepts from the public Blockchain dataset.
- [ACME Retail](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/acme_retail) - The v0.2 showcase: a fictional retailer's finance knowledge exercising Attested Computations (with a deterministic `sql_equality.py` attester), executor skills, and `deprecated` lifecycle states.

## Community Tools

- [okft](https://github.com/PoorvaJ-WW/okft) - Linter and MCP server for OKF bundles (`pip install okft`): `okft lint` validates spec conformance and hygiene (broken links, orphans, timestamps) with CI-friendly exit codes and JSON output; `okft serve` exposes a bundle to any MCP-capable agent as deterministic navigation tools. Apache-2.0.
- [BundleDex](https://bundledex.net) - Directory of 440+ OKF bundles with search, dedup, categorization, and a JSON API for agents.
- [OKF Bundle Generator](https://suganthan.com/okf-generator/) - Free web tool by Suganthan Mohanadasan: paste a URL or sitemap, it crawls up to 100 pages, converts each into a clean OKF concept, links them into a graph, and outputs a downloadable v0.2 bundle (records `generated` and `sources`; `verified` is left to you).
- [SchemaCrawler Scribe](https://www.schemacrawler.com/scribe.html) - Generates structured database documentation directly from live schema metadata in Google Open Knowledge Format (OKF). See [AI-Ready Database Docs You Can Keep in Git](https://dev.to/sualeh/schemacrawler-scribe-google-okf-ai-ready-database-docs-you-can-keep-in-git-2off) for a quick overview.
- [samemind](https://github.com/alexgrebeshok-coder/samemind) - Personal memory for AI coding agents, stored as an OKF bundle: identity, an append-only work-ledger and a kanban board in plain Markdown. `export`/`import` speak the OKF v0.1 wire format directly; zero-dep CLI plus an MCP server, with a 12-engine instruction-file installer.
- [knowledge-mcp](https://github.com/chirag127/knowledge-mcp) - Public MCP server that exposes an OKF bundle over `search`, `read`, `list`, and `related` tools; deployed on Cloudflare Workers with no auth. Points at any OKF v0.1 bundle so any MCP client (Claude Code, Cursor, etc.) can query it as a knowledge graph.
- [okf-gem](https://github.com/serradura/okf-gem) - Ruby harness for a bundle's whole life: an agent skill authors and curates concepts, a CLI and library check them (`validate` for v0.1 conformance, `lint` as a separate non-blocking curation report, ranked `search` for retrieval), and `okf server` / `okf render` publish an interactive graph, live or as one self-contained HTML file. Runs fully locally; `gem install okf` or Docker; [live demo](https://demo.okfgem.com). Apache-2.0.
- [okf-skills](https://github.com/scaccogatto/okf-skills) - OKF toolkit for Claude Code: a plugin, agent skills, and a GitHub Action to author, validate, and visualize bundles. Built on v0.2 — trust tiers, provenance, staleness, and attested computations. MIT.
- [EchoesVault](https://github.com/psinetron/echoes-vault-opencode) - Persistent-memory plugin for OpenCode that stores its vault as an OKF-compliant bundle — one that doubles as a valid Obsidian vault. MIT.
- [claude-mega-brain](https://github.com/guhcostan/claude-mega-brain) - Claude Code plugin that injects an OKF knowledge base at session start, with an agentic benchmark against Obsidian-plus-MCP setups. MIT.
- [OWOX Model Canvas](https://github.com/OWOX/models) - Visual, Miro-like editor for data models that reads and writes OKF as its native format; the repo also publishes real bundles. Apache-2.0.
- [Keepsake](https://github.com/roee-fs/keepsake) - Cloud-native Context Engine implementing the OKF format. Highly available memory solution for fleets of agents cooperating and learning together.

## Articles & Guides

- [How the Open Knowledge Format can improve data sharing](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) - The official Google Cloud announcement (Sam McVeety & Amir Hormati). Start here.
- [OKF v0.2 adds trust signals](https://cloud.google.com/blog/products/data-analytics/okf-v0-2-adds-trust-signals) - The official v0.2 announcement (July 2026): why agent-maintained corpora need provenance, trust, freshness, lifecycle, and attestation answerable straight from frontmatter.
- [Introducing the Google Cloud Knowledge Catalog](https://cloud.google.com/blog/products/data-analytics/introducing-the-google-cloud-knowledge-catalog) - The companion product OKF was designed alongside; Knowledge Catalog produces and consumes OKF as its open, portable format.
- [OKF, by Marie Haynes](https://www.mariehaynes.com/okf/) - Why OKF matters for SEO/AI: the shift from "being found by search engines" to "making knowledge accessible so agents can act on it."
- [Build an OKF brain like mine!](https://www.mariehaynes.com/build-an-okf-brain-like-mine/) - Marie Haynes' follow-up: a walkthrough of her personal OKF brain (concepts, entities, playbooks, references, systems), with prompts to build your own.
- [Open Knowledge Format (OKF): Google's New Markdown Format for AI Agents](https://suganthan.com/blog/open-knowledge-format/) - Practical explainer by Suganthan Mohanadasan, updated for v0.2's trust and provenance fields.
- [Google shipped an open format (OKF). My site already spoke it.](https://suganthan.com/notes/google-shipped-okf/) - Hands-on notes from building one of the first community bundles.
- [Google Cloud Announces The Open Knowledge Format](https://www.searchenginejournal.com/google-cloud-announces-the-open-knowledge-format/579253/) - Search Engine Journal coverage.
- [What Is Google's Open Knowledge Format (OKF)? A Plain-English Guide for Site Owners](https://nexterwp.com/blog/open-knowledge-format/) - Beginner-friendly guide aimed at website owners.
- [Open Knowledge Format (OKF): Google AI Agent Standard](https://www.explainx.ai/blog/google-open-knowledge-format-okf-ai-agents-2026) - Overview of OKF in the context of agentic AI.
- [Open Knowledge Format: What Google Actually Shipped](https://www.iloveseo.net/the-open-knowledge-format-what-google-really-shipped/) - Gianluca Fiorelli's long-form analysis: OKF as enterprise agent memory rather than an SEO play, with early-adopter interviews and the ARD relationship mapped out.
- [Composing OKF and ARD on specification.website](https://joost.blog/okf-ard/) - Joost de Valk implements both specs on one site, hits real field-name conflicts between them, and files the issues upstream.

## Background & Origins

- [LLM Wiki, by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) - The "LLM-wiki" pattern that OKF formalizes. OKF's reserved `index.md` and `log.md` files and its raw-sources/wiki/schema layering trace directly back to this gist.
- [As We May Think, by Vannevar Bush (1945)](https://en.wikipedia.org/wiki/As_We_May_Think) - The Memex essay Karpathy cites as the deeper ancestor: a personal, curated knowledge store where the links between documents matter as much as the documents themselves.

## Built on the LLM-Wiki Pattern

Community implementations of the LLM-wiki pattern that OKF formalizes. These are not OKF-native, but they share its "knowledge as interlinked Markdown that an agent maintains" philosophy.

- [AutoSci](https://github.com/skyllwt/AutoSci) - Full-lifecycle AI research platform built on the LLM-wiki vision, with 28 agent skills (slash commands in Claude Code, with Codex and OpenCode adaptations) for ingesting and synthesizing papers.
- [karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki) - Agent Skills-compatible LLM wiki for Claude Code, Cursor, and Codex that ingests sources, compiles wiki pages, answers with citations, and lints for consistency.
- [llmwiki](https://github.com/lucasastorian/llmwiki) - Open-source implementation: upload documents, connect Claude via MCP, and have the agent write and maintain the wiki.
- [Synto](https://github.com/kytmanov/synto) - Local-first wiki builder using a two-tier Ollama pipeline (small model extracts concepts, larger model writes cross-linked articles) on consumer hardware.
- [llm_wiki](https://github.com/nashsu/llm_wiki) - Cross-platform desktop app that turns documents into an organized, interlinked knowledge base automatically.

## Related Formats & Concepts

- [Agentic Resource Discovery (ARD)](https://github.com/ards-project/ard-spec) - Google-announced sister spec, published days after OKF: federated catalogs advertise resources — OKF bundles included — so agents can discover them. OKF packages knowledge; ARD finds it.
- [llms.txt](https://llmstxt.org/) - A proposed standard for a Markdown file that helps LLMs use a website's content.
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) - An open protocol for connecting AI agents to tools and data sources.
- [AGENTS.md](https://agents.md/) - A simple, open convention for giving coding agents project instructions.
- [Obsidian](https://obsidian.md/) - Markdown vaults with backlinks; a close cousin of the OKF directory-of-notes model.
- [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) - Query language over Markdown frontmatter, useful for building dynamic views from OKF-style structured fields.
- [Marp](https://marp.app/) - Markdown presentation ecosystem; turns Markdown knowledge files into slide decks.

## Community

- [Google Cloud Tech on X](https://x.com/GoogleCloudTech/status/2067012903337664886) - The launch announcement.
- [Google proposes Open Knowledge Format based on Markdown (Hacker News)](https://news.ycombinator.com/item?id=48517735) - The launch discussion: skepticism, prior art, and alternatives.
- [awesome-okf (Chinese)](https://github.com/yzfly/awesome-okf) - Chinese-language sister list, bundled with Feishu, Obsidian, Notion, and GitHub-to-OKF converters.

## Contributing

Contributions are welcome! Please read the [contribution guidelines](CONTRIBUTING.md) first. The OKF ecosystem is brand new — if you build a producer, consumer, viewer, or bundle, add it here.
