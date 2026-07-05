# Awesome OKF [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for the **Open Knowledge Format (OKF)** — Google's open, vendor-neutral spec for representing knowledge as Markdown files with YAML frontmatter that AI agents can read without custom integrations. This list practices what it catalogs: it is itself published as a conformant OKF bundle (see below).

OKF was announced by Google Cloud in June 2026. It formalizes the "LLM-wiki" pattern into a portable, interoperable format: knowledge lives as plain Markdown files with structured frontmatter (`type`, `title`, `description`, `resource`, `tags`, `timestamp`) and cross-links that form a knowledge graph. No SDK, no runtime, no lock-in — just files you can ship in a tarball, host in Git, or mount on a filesystem.

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

A bundle is a directory tree of Markdown files, and the directory structure is independent of the domain. The essentials from the v0.1 spec:

| Aspect             | Rule                                                                                                                         |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| Concept            | One Markdown file: YAML frontmatter plus a free-form Markdown body.                                                          |
| Required field     | `type` — a non-empty string identifying the kind of concept.                                                                 |
| Recommended fields | `title`, `description`, `resource` (a URI for the underlying asset), `tags`, `timestamp` (ISO 8601).                         |
| Reserved filenames | `index.md` (directory listing for progressive disclosure) and `log.md` (update history); every other `.md` is a concept.     |
| Links              | A link from concept A to B asserts a relationship; use bundle-relative paths (starting with `/`) or ordinary relative paths. |
| Conformance        | Every non-reserved `.md` file has parseable frontmatter with a non-empty `type`.                                             |
| Consumer tolerance | Consumers must tolerate missing optional fields, unknown `type` values, broken links, and a missing `index.md`.              |
| Extensions         | Producers may add custom keys; consumers must preserve unknown fields.                                                       |

> This list is also published as a conformant OKF v0.1 bundle under [`bundle/`](bundle), generated from this README by [`scripts/build-okf-bundle.mjs`](scripts/build-okf-bundle.mjs).

## Specification

- [OKF v0.1 Specification (SPEC.md)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) - The one-page, universal, vendor-neutral spec, including conformance criteria and reserved filenames.
- [GoogleCloudPlatform/knowledge-catalog `okf/`](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf) - The home repository for the format, reference code, and samples.
- [OKF README](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/README.md) - Overview of OKF philosophy, installation, and usage.

## Official Tools & Reference Implementations

- [Reference Agent](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/src/reference_agent) - Python implementation (built on Google's Agent Development Kit) that produces and visualizes OKF bundles: it drafts OKF documents from a BigQuery source, enriches them with web-crawled citations, and ships a `viewer/generator.py` that renders the bundle as a self-contained, interactive graph.
- [Knowledge Catalog Enrichment toolbox](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/toolbox/enrichment) - A ready-to-use agent and customizable harness (TypeScript) to produce, evolve, and maintain metadata in Knowledge Catalog. Includes a server that exposes a Markdown fileset as an MCP server.
- [Knowledge Catalog mdcode](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/toolbox/mdcode) - Manage metadata as source-code artifacts, with git-style pull/push sync between OKF Markdown and BigQuery / Dataplex / Knowledge Catalog.

## Sample Bundles

Three conformant, ready-to-browse bundles built from public BigQuery datasets, each with a self-contained interactive `viz.html` graph viewer:

- [GA4 e-commerce](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/ga4) - Google Analytics 4 e-commerce metadata.
- [Stack Overflow](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/stackoverflow) - Schema graph for the public Q&A dataset.
- [Bitcoin](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles/crypto_bitcoin) - On-chain concepts from the public Blockchain dataset.

## Community Tools

- [BundleDex](https://bundledex.net) - Directory of 227+ OKF bundles with search, dedup, categorization, and a JSON API for agents.
- [OKF Bundle Generator](https://suganthan.com/okf-generator/) - Free web tool by Suganthan Mohanadasan: paste a URL or sitemap, it crawls up to 100 pages, converts each into a clean OKF concept, links them into a graph, and outputs a downloadable bundle. A companion WordPress plugin generates a bundle in one click and keeps it in sync on every publish.

## Articles & Guides

- [How the Open Knowledge Format can improve data sharing](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) - The official Google Cloud announcement (Sam McVeety & Amir Hormati). Start here.
- [Introducing the Google Cloud Knowledge Catalog](https://cloud.google.com/blog/products/data-analytics/introducing-the-google-cloud-knowledge-catalog) - The companion product OKF was designed alongside; Knowledge Catalog produces and consumes OKF as its open, portable format.
- [OKF, by Marie Haynes](https://www.mariehaynes.com/okf/) - Why OKF matters for SEO/AI: the shift from "being found by search engines" to "making knowledge accessible so agents can act on it."
- [Open Knowledge Format (OKF): Google's New Markdown Format for AI Agents](https://suganthan.com/blog/open-knowledge-format/) - Practical explainer by Suganthan Mohanadasan.
- [Google shipped an open format (OKF). My site already spoke it.](https://suganthan.com/notes/google-shipped-okf/) - Hands-on notes from building one of the first community bundles.
- [Google Cloud Announces The Open Knowledge Format](https://www.searchenginejournal.com/google-cloud-announces-the-open-knowledge-format/579253/) - Search Engine Journal coverage.
- [What Is Google's Open Knowledge Format (OKF)? A Plain-English Guide for Site Owners](https://nexterwp.com/blog/open-knowledge-format/) - Beginner-friendly guide aimed at website owners.
- [Open Knowledge Format (OKF): Google AI Agent Standard](https://www.explainx.ai/blog/google-open-knowledge-format-okf-ai-agents-2026) - Overview of OKF in the context of agentic AI.

## Background & Origins

- [LLM Wiki, by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) - The "LLM-wiki" pattern that OKF formalizes. OKF's reserved `index.md` and `log.md` files and its raw-sources/wiki/schema layering trace directly back to this gist.
- [As We May Think, by Vannevar Bush (1945)](https://en.wikipedia.org/wiki/As_We_May_Think) - The Memex essay Karpathy cites as the deeper ancestor: a personal, curated knowledge store where the links between documents matter as much as the documents themselves.

## Built on the LLM-Wiki Pattern

Community implementations of the LLM-wiki pattern that OKF formalizes. These are not OKF-native, but they share its "knowledge as interlinked Markdown that an agent maintains" philosophy.

- [AutoSci](https://github.com/skyllwt/AutoSci) - Full-lifecycle AI research platform built on the LLM-wiki vision, powered by Claude Code, with 30+ slash commands for ingesting and synthesizing papers.
- [karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki) - Agent Skills-compatible LLM wiki for Claude Code, Cursor, and Codex that ingests sources, compiles wiki pages, answers with citations, and lints for consistency.
- [llmwiki](https://github.com/lucasastorian/llmwiki) - Open-source implementation: upload documents, connect Claude via MCP, and have the agent write and maintain the wiki.
- [Synto](https://github.com/kytmanov/synto) - Local-first wiki builder using a two-tier Ollama pipeline (small model extracts concepts, larger model writes cross-linked articles) on consumer hardware.
- [llm_wiki](https://github.com/nashsu/llm_wiki) - Cross-platform desktop app that turns documents into an organized, interlinked knowledge base automatically.

## Related Formats & Concepts

- [llms.txt](https://llmstxt.org/) - A proposed standard for a Markdown file that helps LLMs use a website's content.
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) - An open protocol for connecting AI agents to tools and data sources.
- [AGENTS.md](https://agents.md/) - A simple, open convention for giving coding agents project instructions.
- [Obsidian](https://obsidian.md/) - Markdown vaults with backlinks; a close cousin of the OKF directory-of-notes model.
- [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) - Query language over Markdown frontmatter, useful for building dynamic views from OKF-style structured fields.
- [Marp](https://marp.app/) - Markdown presentation ecosystem; turns Markdown knowledge files into slide decks.

## Community

- [Google Cloud Tech on X](https://x.com/GoogleCloudTech/status/2067012903337664886) - The launch announcement.

## Contributing

Contributions are welcome! Please read the [contribution guidelines](CONTRIBUTING.md) first. The OKF ecosystem is brand new — if you build a producer, consumer, viewer, or bundle, add it here.
