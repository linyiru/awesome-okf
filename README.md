# Awesome OKF [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for the **Open Knowledge Format (OKF)** — an open, vendor-neutral specification for representing knowledge as a directory of Markdown files with YAML frontmatter, so AI agents (and humans) can read it without custom integrations.

OKF was announced by Google Cloud in June 2026. It formalizes the "LLM-wiki" pattern into a portable, interoperable format: knowledge lives as plain Markdown files with structured frontmatter (`type`, `title`, `description`, `resource`, `tags`, `timestamp`) and cross-links that form a knowledge graph. No SDK, no runtime, no lock-in — just files you can ship in a tarball, host in Git, or mount on a filesystem.

## Contents

- [Specification](#specification)
- [Official Tools & Reference Implementations](#official-tools--reference-implementations)
- [Sample Bundles](#sample-bundles)
- [Community Tools](#community-tools)
- [Articles & Guides](#articles--guides)
- [Background & Origins](#background--origins)
- [Related Formats & Concepts](#related-formats--concepts)
- [Community](#community)

## Specification

- [OKF v0.1 Specification (SPEC.md)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) - The one-page, universal, vendor-neutral spec, including conformance criteria and reserved filenames.
- [GoogleCloudPlatform/knowledge-catalog `okf/`](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf) - The home repository for the format, reference code, and samples.
- [OKF README](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/README.md) - Overview of OKF philosophy, installation, and usage.

## Official Tools & Reference Implementations

- [Reference Agent](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/src/reference_agent) - Reference implementation for producing and visualizing OKF bundles. Includes an enrichment agent that walks BigQuery datasets and drafts OKF documents, plus a static HTML visualizer that renders a bundle as a self-contained, interactive Cytoscape.js graph with no backend required.
- [Knowledge Catalog Toolbox examples](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/toolbox/mdcode/demo) - Examples of integrating OKF with Google Cloud's Knowledge Catalog.

## Sample Bundles

- [Sample OKF bundles](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles) - Three ready-to-browse bundles built from public datasets, each with an interactive `viz.html`: GA4 e-commerce, Stack Overflow, and Bitcoin / Blockchain.

## Community Tools

- [OKF Bundle Generator](https://suganthan.com/okf-generator/) - Free web tool by Suganthan Mohanadasan: paste a URL or sitemap, it crawls up to 100 pages, converts each into a clean OKF concept, links them into a graph, and outputs a downloadable bundle. A companion WordPress plugin generates a bundle in one click and keeps it in sync on every publish.

## Articles & Guides

- [How the Open Knowledge Format can improve data sharing](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) - The official Google Cloud announcement (Sam McVeety & Amir Hormati). Start here.
- [OKF, by Marie Haynes](https://www.mariehaynes.com/okf/) - Why OKF matters for SEO/AI: the shift from "being found by search engines" to "making knowledge accessible so agents can act on it."
- [Open Knowledge Format (OKF): Google's New Markdown Format for AI Agents](https://suganthan.com/blog/open-knowledge-format/) - Practical explainer by Suganthan Mohanadasan.
- [Google shipped an open format (OKF). My site already spoke it.](https://suganthan.com/notes/google-shipped-okf/) - Hands-on notes from building one of the first community bundles.
- [Google Cloud Announces The Open Knowledge Format](https://www.searchenginejournal.com/google-cloud-announces-the-open-knowledge-format/579253/) - Search Engine Journal coverage.
- [What Is Google's Open Knowledge Format (OKF)? A Plain-English Guide for Site Owners](https://nexterwp.com/blog/open-knowledge-format/) - Beginner-friendly guide aimed at website owners.
- [Open Knowledge Format (OKF): Google AI Agent Standard](https://www.explainx.ai/blog/google-open-knowledge-format-okf-ai-agents-2026) - Overview of OKF in the context of agentic AI.

## Background & Origins

- [How To Build an LLM Wiki, by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) - The "LLM-wiki" pattern that OKF formalizes.

## Related Formats & Concepts

- [llms.txt](https://llmstxt.org/) - A proposed standard for a Markdown file that helps LLMs use a website's content.
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) - An open protocol for connecting AI agents to tools and data sources.
- [AGENTS.md](https://agents.md/) - A simple, open convention for giving coding agents project instructions.
- [Obsidian](https://obsidian.md/) - Markdown vaults with backlinks; a close cousin of the OKF directory-of-notes model.

## Community

- [Google Cloud Tech on X](https://x.com/GoogleCloudTech/status/2067012903337664886) - The launch announcement.

## Contributing

Contributions are welcome! Please read the [contribution guidelines](CONTRIBUTING.md) first. The OKF ecosystem is brand new — if you build a producer, consumer, viewer, or bundle, add it here.
