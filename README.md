# ipg-docs

Documentation site for [`sanjaysgk/ipg`](https://github.com/sanjaysgk/ipg) — the nf-core Nextflow port of the immunopeptidogenomics cryptic peptide database construction pipeline (Scull et al. 2021, *Mol Cell Proteomics* 20, 100143).

This is a [Mintlify](https://mintlify.com) site. The rendered version is the canonical reference for installing, running, and understanding the IPG pipeline.

## Quick links

- **Pipeline source:** https://github.com/sanjaysgk/ipg
- **Founding paper:** https://doi.org/10.1016/j.mcpro.2021.100143
- **Original C tools:** https://github.com/sanjaysgk/immunopeptidogenomics

## Local preview on Monash M3

The dev server runs on a Monash M3 interactive node and tunnels back to your laptop browser. Full walkthrough: see [`docs/dev-starter.mdx`](docs/dev-starter.mdx) (which will become [`get-started/dev-preview`](get-started/dev-preview.mdx) on the rendered site).

```bash
# On a Monash M3 interactive node (smux):
cd /path/to/ipg-docs
pixi add nodejs=22.*       # one-time
pixi run npx mint dev      # serves on localhost:3000

# On your laptop, in a separate terminal — replace m3sNNN with `hostname` from the smux session:
ssh -L 3000:m3sNNN:3000 sson0030@m3.massive.org.au

# Then open http://localhost:3000 in your laptop browser.
```

## Repo layout

```
ipg-docs/
├── docs.json              # Mintlify site config — single source of truth for nav/theme/branding
├── index.mdx              # Landing page
├── get-started/           # Onboarding (install, quickstart, dev preview, first run)
├── pipeline/              # Pipeline reference: overview + 6 subworkflow pages
├── api-reference/         # CLI parameters, channel I/O, profiles, outputs (renamed "Reference" tab)
├── tutorials/             # End-to-end recipes (Monash M3, Docker, AWS Batch)
├── operations/            # Troubleshooting, FAQ, performance
├── background/            # Cryptic peptide biology, methods, citations
├── contributing/          # Dev workflow for contributors to this docs site
├── essentials/, ai-tools/, snippets/, api-reference/endpoint/  # ← Mintlify starter examples (Cookbook tab)
├── logo/, images/, favicon.svg  # Brand assets (logos generated via nf-core/tools)
├── docs/                  # Working notes (currently the dev preview walkthrough)
└── pixi.toml, pixi.lock   # Per-project Node 22 environment
```

The starter pages from the original Mintlify template are preserved as a **Cookbook** tab — they are kept as a living reference for how to use Mintlify components while writing IPG content. They are not deleted.

## Documentation rollout phases

The site is being built in phases — see `.claude/PLAN.md` (gitignored, working memory). Current state:

| Phase | Status |
|---|---|
| **Phase 0** — Foundation (git, pixi, gitignore, dev preview docs) | ✅ Done |
| **Phase 1** — Rebrand & augment (docs.json, IA scaffold, logos, landing) | ⏳ In progress |
| **Phase 2** — IA placeholders for every page | ✅ Done |
| **Phase 3** — Onboarding (get-started/* real content) | ⏳ Next |
| **Phase 4** — Pipeline reference (real subworkflow pages) | ⏳ Pending pipeline Day 3+4 |
| **Phase 5** — API reference (parameters & channels real content) | ⏳ |
| **Phase 6** — Tutorials, ops, background, contributing | ⏳ |
| **Phase 7** — Polish (broken-link, a11y, OG cards, deploy) | ⏳ |

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the docs writing workflow, branching model, commit conventions, and the document type contract.

## License

[`LICENSE`](LICENSE) — same license as the upstream Mintlify starter (MIT).
