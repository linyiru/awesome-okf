# Contribution Guidelines

Thanks for helping make **Awesome OKF** better! Please keep submissions on-topic and high-quality.

## How to contribute

1. Fork this repository.
2. Add your link to the most relevant section in `README.md`, keeping entries in a sensible order.
3. Regenerate the OKF bundle so it stays in sync: `node scripts/build-okf-bundle.mjs`, then commit the changes under `bundle/`. CI fails if the bundle is out of date.
4. Open a pull request with a short note explaining why the resource belongs here.

## Entry format

Use this format, with a concise, factual description (not marketing copy):

```
- [Name](https://example.com) - One-line description of what it is and why it's useful.
```

- Use a hyphen surrounded by spaces (` - `) between the link and the description, as required by `awesome-lint`.
- Start the description with a capital letter and end it with a period.
- Keep it neutral and specific. Avoid superlatives like "the best".

## What belongs here

- The OKF specification and official Google Cloud resources.
- Producers (tools that generate OKF bundles) and consumers (viewers, search indexes, agents that read OKF).
- Real OKF bundles people can browse or reuse.
- Substantive articles, guides, and talks about OKF.
- Closely related formats and standards (kept in the "Related" section).

## What doesn't

- Dead links, paywalled-only content with no public value, or thin SEO spam.
- Duplicate entries — check the list first.
- Self-promotion of low-quality or unmaintained projects.

## Quality bar

- Links must work and point to a stable URL.
- Projects should be functional and, ideally, maintained.
- One resource per pull request makes review easier (but related batches are fine).

By contributing, you agree that your contributions are licensed under the [CC0 license](LICENSE).
