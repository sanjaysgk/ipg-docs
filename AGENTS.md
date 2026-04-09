# Agents

Project-specific instructions for AI coding agents (Claude Code, Cursor, Windsurf, Aider, etc.) working on `ipg-docs`.

## About this project

- **Subject:** the [`sanjaysgk/ipg`](https://github.com/sanjaysgk/ipg) nf-core Nextflow pipeline (cryptic peptide database construction). The sister pipeline repo lives at `../ipg` relative to this one.
- **Stack:** [Mintlify](https://mintlify.com) — MDX files with YAML frontmatter, configured by `docs.json`. Local preview via `pixi run npx mint dev`.
- **Founding paper:** [Scull et al. 2021, *Mol Cell Proteomics*](https://doi.org/10.1016/j.mcpro.2021.100143). Cite throughout.
- **Working memory** (gitignored): `.claude/RULES.md`, `.claude/PLAN.md`, `.claude/STATUS.md`, `.claude/ARCHITECTURE.md`, `.claude/MEMORY.md`, `.claude/PIPELINE_STEPS.md`, `.claude/DOC_TYPES.md`, `.claude/API_REFERENCE_PLAN.md`. Read these at the start of every session.
- **Mintlify reference** (also gitignored, cached locally): `.claude/mintlify-skill/` — official component / configuration / navigation / API-docs reference from `mintlify/mintlify-claude-plugin`. **Read these for authoritative syntax instead of guessing.**

## Hard rules (non-negotiable)

1. **NEVER add Claude / AI attribution to commits or PR bodies.** No `Co-Authored-By: Claude`, no `🤖 Generated with Claude Code`, no AI co-authoring of any kind. This is an academic project destined for citation.
2. **NEVER commit `.claude/`, `CLAUDE.md`, `.mintlify/`, `node_modules/`, or `.pixi/`.** All gitignored. Don't remove the gitignore lines.
3. **NEVER push to `main` without explicit user confirmation.** Topic work on `dev/<area>` branches.
4. **NEVER `git config --global`** — local-only `sanjaysgk <44039457+sanjaysgk@users.noreply.github.com>`.
5. **NEVER delete starter example pages** (`essentials/*`, `ai-tools/*`, `api-reference/endpoint/*`, `quickstart.mdx`, `development.mdx`, `snippets/snippet-intro.mdx`) without explicit per-file approval. They are the Cookbook tab — the only living reference for how to use Mintlify components in this project.
6. **NEVER edit `../ipg/`** from this repo's working session. The pipeline is upstream truth — read it, don't mutate it.

## Style preferences

- **Active voice, second person.** "You run…" not "The pipeline can be run…"
- **Sentence case for headings.** "Getting started", not "Getting Started".
- **`code` formatting** for file names, commands, paths, parameter names, channel names, tool versions.
- **Bold for UI elements:** Click **Settings**.
- **No marketing language.** No "powerful", "seamless", "cutting-edge".
- **Cite every claim** with a link or a `background/citations.mdx` reference.
- **Always show the exact command, version, profile.** Reproducibility over brevity.
- **Mintlify components over raw HTML.** Use `<Steps>`, `<Tabs>`, `<Columns>` + `<Card>`, `<AccordionGroup>`, `<Note>`/`<Warning>`/`<Tip>`/`<Info>`/`<Check>`/`<Danger>`, `<CodeGroup>`, `<ParamField>`, `<ResponseField>`, `<Frame>`, Mermaid fences. **Verify component syntax against `.claude/mintlify-skill/reference/components.md` before writing.** Note: use `<Columns cols={2}>` to wrap `<Card>`s, NOT `<CardGroup>`.
- **Internal links are root-relative without extensions:** `/pipeline/align-qc`, never `/pipeline/align-qc.mdx` or `../pipeline/align-qc`.
- **All code blocks must have a language tag.** Bare ` ``` ` blocks fail `mint validate`.

## Document type contract

Every MDX page declares its `docType` in frontmatter. The 8 types are: **landing, conceptual, how-to, reference, pipeline-stage, troubleshoot, changelog, cookbook**. Each type has a fixed component palette — see `.claude/DOC_TYPES.md` for the matrix. Reviewers will reject pages that smuggle the wrong components into the wrong type.

## kescull tool naming

The custom C tools authored by Katherine E. Scull (`curate_vcf`, `revert_headers`, `alt_liftover`, `triple_translate`, `squish`, plus the not-yet-ported `filter_FPKM`, `origins`, `msDot`) are referred to as **"kescull tools"** in this docs site. Always credit Katherine when referencing them; preserve the C lineage. They are never to be rewritten in another language.

## Scope boundary

`sanjaysgk/ipg` covers steps 1–31 of the legacy bash pipeline — **database construction only**. The MS-search half (`origins`, `db_compare`, `msDot`, `filter_FPKM`) is the planned [`ipg-origins` sister pipeline](/background/related-work). When in doubt about scope, default to `ipg-origins` and add a roadmap entry rather than expanding `ipg`.

## Before you start

1. Read `docs.json` to understand the current navigation.
2. Read `.claude/STATUS.md` for the current sprint state.
3. Read 2–3 existing pages similar to what you're writing to match voice and structure.
4. Read the relevant `.claude/mintlify-skill/reference/*.md` file for authoritative syntax.
5. Run `pixi run npx mint dev` if you're touching anything visual.

## Mintlify gotchas

- `<Columns cols={2}>` to wrap cards, not `<CardGroup>`.
- Internal links: root-relative, no `.mdx` extension.
- Code blocks need language tags or `mint validate` will fail.
- Adding a page also requires registering it in `docs.json` — Mintlify won't pick it up otherwise.
- Images need alt text or `mint a11y` will flag them.
