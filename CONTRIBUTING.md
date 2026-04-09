# Contributing to ipg-docs

Thanks for helping document the IPG pipeline. This is the Mintlify documentation site for [`sanjaysgk/ipg`](https://github.com/sanjaysgk/ipg) — the nf-core Nextflow port of the cryptic peptide database construction pipeline first published in Scull et al. 2021.

This guide is for contributors to **the documentation site itself**. If you want to contribute to the pipeline, see [`sanjaysgk/ipg`'s CONTRIBUTING.md](https://github.com/sanjaysgk/ipg/blob/main/.github/CONTRIBUTING.md).

## Local development

You'll need Node 22+. The recommended setup is per-project via `pixi`:

```bash
git clone https://github.com/sanjaysgk/ipg-docs.git
cd ipg-docs
pixi add nodejs=22.*       # only the first time
pixi run npx mint dev      # serves on http://localhost:3000
```

If you're working on Monash M3, the dev server runs on an interactive node and tunnels back to your laptop — see [`docs/dev-starter.mdx`](docs/dev-starter.mdx) for the full walkthrough.

Before opening a PR:

```bash
pixi run npx mint validate         # strict build, exits non-zero on warnings
pixi run npx mint broken-links     # check internal + anchor links
pixi run npx mint a11y             # accessibility (alt text, contrast)
```

## Branch and commit conventions

- **Branches:** all work on `dev/<area>` topic branches off `main`. Examples:
  - `dev/rebrand` — branding / theme / nav structure
  - `dev/onboarding` — `get-started/*` real content
  - `dev/pipeline-pages` — `pipeline/*` real content
  - `dev/api-reference` — `api-reference/*` real content
- **`main` is protected.** Never commit directly. PR review required.
- **Atomic commits.** One logical change per commit. If your subject can't fit in one sentence, split it.
- **Commit message format:** `<area>: <imperative summary>` — examples:
  - `branding: rebrand from mintlify starter to IPG`
  - `pipeline: write real align-qc subworkflow page`
  - `reference: add reference-genome parameter page`
  - `troubleshooting: add Monash STAR memory FAQ entry`
- **No emoji in commit messages.** No co-author trailers, no `Generated with` lines, no AI attribution. Commits are authored as the contributor only.
- **Stage explicit files** with `git add <file>`. Never `git add .` or `git add -A` — too easy to accidentally include build artifacts or working memory files.
- **Never run `git config --global`** — git identity is set locally per clone.

## The 8 document types

Every page on the site declares its type so the structure stays consistent. Pick the right type for what you're writing:

| Type | What it does | Lives in | Allowed components |
|---|---|---|---|
| **landing** | Routes the reader to the next page | `index.mdx`, tab landings | `<Card>`, `<Columns>`, Mermaid, `<Update>` |
| **conceptual** | Explains the science / why | `background/*`, `pipeline/overview` | callouts, Mermaid, KaTeX, citations |
| **how-to** | Step-by-step recipe | `get-started/*`, `tutorials/*` | `<Steps>`, `<Tabs>`, `<CodeGroup>`, `<Check>`, `<AccordionGroup>` |
| **reference** | Look up a parameter / channel / file | `api-reference/*` | `<ParamField>`, `<ResponseField>`, `<Expandable>`, `<CodeGroup>` |
| **pipeline-stage** | Document one subworkflow end-to-end | `pipeline/<subworkflow>.mdx` | fixed template — see below |
| **troubleshoot** | Match a symptom to a fix | `operations/troubleshooting`, `operations/faq` | `<AccordionGroup>` of accordions |
| **changelog** | Per-release notes | `contributing/changelog` | `<Update>` blocks only |
| **cookbook** | Mintlify component examples | `essentials/*`, `ai-tools/*`, `api-reference/endpoint/*`, `quickstart`, `development` | all components |

The full contract — including the component-to-doc-type matrix that says e.g. `<Steps>` is banned in landing pages and `<ParamField>` is banned in tutorials — lives in `.claude/DOC_TYPES.md` (working memory, gitignored).

## Frontmatter contract

Every page must declare:

```yaml
---
title: "Page title in sentence case"
description: "One sentence — shows up in search and OG cards"
icon: "lucide-icon-name"
sidebarTitle: "Short title"   # optional, only if title is too long
docType: "pipeline-stage"     # one of the 8 types above
audience: "scientist"          # one of: scientist, bioinformatician, reviewer, contributor, internal
---
```

`docType:` and `audience:` aren't Mintlify-native — they're our discipline. They allow grep audits during polish.

## Writing style

- **Active voice. Second person.** "You run the pipeline with…" not "the pipeline can be run with…"
- **One idea per sentence.** Short sentences over long ones.
- **Sentence case for headings.** "Running on Monash M3" — not "Running On Monash M3".
- **Bold for UI elements** (`Click **Settings**`); ` `code` ` for files, paths, commands, params, channel names.
- **No marketing fluff.** No "powerful", "seamless", "cutting-edge", "state-of-the-art".
- **Cite every claim.** Tools, methods, and biological assertions get a citation; the citation lives in `background/citations.mdx`.
- **Reproducibility over brevity.** Always show the exact command, the exact version, the exact profile.

## Preserve the Cookbook tab

The Mintlify starter pages (`essentials/*`, `ai-tools/*`, `api-reference/endpoint/*`, `quickstart`, `development`) are kept as a **Cookbook** in the docs.json nav. They are the only working reference for how to use every Mintlify component. **Do not delete them** without explicit per-file approval.

## Cross-repo dependency on the pipeline

When `sanjaysgk/ipg` changes — new param, new subworkflow, renamed channel — this docs site needs a sync sweep. The signal is the pipeline's `STATUS.md` (gitignored in that repo). If you make changes to the pipeline, also open a PR here to keep the docs in step.

## Need help?

- File an issue: https://github.com/sanjaysgk/ipg-docs/issues
- Discussion of the underlying pipeline: https://github.com/sanjaysgk/ipg/issues
