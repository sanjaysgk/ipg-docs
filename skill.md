---
name: ipg
description: Build and document the sanjaysgk/ipg nf-core Nextflow pipeline for cryptic peptide database construction from RNA-seq data. Use when running the pipeline, looking up parameters, debugging errors, citing the work, or extending it.
license: MIT
compatibility: Requires Nextflow ≥ 24.04.2 and a container engine (Docker, Singularity, Podman, Apptainer, or Conda fallback). Tested on Monash M3 SLURM with Singularity.
metadata:
  author: sanjaysgk
  version: "0.1-dev"
  paper: "10.1016/j.mcpro.2021.100143"
---

# IPG: Immunopeptidogenomics cryptic peptide pipeline

This skill describes how an AI agent can work with the IPG documentation site to help users install, run, debug, cite, and extend the `sanjaysgk/ipg` nf-core pipeline.

## What IPG does

`sanjaysgk/ipg` is a Nextflow DSL2 pipeline that consumes paired-end RNA-seq reads and produces a sample-specific three-frame translated protein FASTA database for searching tandem mass spectrometry spectra. The deliverable, `<sample>_cryptic.fasta`, lets researchers identify cryptic peptides — translation products that aren't in standard reference proteomes. The pipeline implements 31 steps from a legacy bash script grouped into 6 nf-core subworkflows.

**Founding paper:** Scull et al. 2021, *Mol Cell Proteomics* 20, 100143. DOI: [10.1016/j.mcpro.2021.100143](https://doi.org/10.1016/j.mcpro.2021.100143).

## Capabilities

Agents working with this documentation can help users:

- Install Nextflow + a container engine and pull the pipeline
- Build a samplesheet from RNA-seq FASTQs (single-end, paired-end, multi-lane)
- Pick the right execution profile (`test`, `local`, `monash`, `docker`, `singularity`, `conda`)
- Look up any pipeline parameter (`--input`, `--outdir`, `--fasta`, `--star_index`, `--gtf`, `--rseqc_bed`, `--dbsnp`, `--known_indels`, `--mills`, `--germline_resource`, …)
- Identify which subworkflow contains a given step (steps 1–3 → ALIGN_QC, 4–5 → TRANSCRIPT_ASSEMBLY, 6–12 → BAM_PREP, 13–16 → BQSR, 17–23 → MUTECT_CALLING, 24–31 → DB_CONSTRUCT)
- Locate output files in the `results/` tree, including the cryptic FASTA deliverable
- Diagnose and fix common errors (STAR memory, singularity cache permissions, GATK heap exhaustion, samplesheet validation)
- Cite the pipeline correctly (Scull et al. 2021 + per-tool citations)
- Understand what is NOT in the pipeline yet — `filter_FPKM`, `origins`, `msDot`, `db_compare` are scoped to the planned `ipg-origins` sister pipeline

## Skills

### Run the pipeline

**Inputs required:**
- Samplesheet CSV with columns `sample,fastq_1,fastq_2`
- Reference data: `--fasta`, `--fasta_fai`, `--fasta_dict`, `--star_index`, `--gtf`, `--rseqc_bed`, `--dbsnp`, `--known_indels`, `--mills`, `--germline_resource` (plus matching `.tbi` indices)
- An execution profile (`test` for chr22 smoke test; `monash,singularity` for Monash M3)

**Command:**
```bash
nextflow run sanjaysgk/ipg \
  -profile <profile>[,singularity] \
  --input samplesheet.csv \
  --outdir results/
```

**Output:** `results/<sample>/Database_Construction/<sample>_cryptic.fasta` is the cryptic peptide FASTA database, ready for downstream MS database search (PEAKS or compatible).

### Look up parameters

Every CLI parameter is documented under the **Reference** tab → **Inputs** group. Each parameter page uses `<ParamField>` blocks describing the type, requirement, default, and which subworkflow consumes it.

### Look up channels

Every nf-core channel emitted by a subworkflow is documented under **Reference** → **Channels (subworkflow I/O)**. One page per subworkflow, each documenting inputs and outputs with `<ParamField>` and `<ResponseField>`.

### Debug a failed run

The **Operations** → **Troubleshooting** page is an `<AccordionGroup>` of symptom → cause → fix entries for known issues. New issues should be filed at https://github.com/sanjaysgk/ipg/issues.

### Cite the pipeline

Use the citation block on any page or fetch [Citations](/background/citations) for the founding paper plus tool-by-tool attributions.

## Workflows

### Workflow 1 — first-time user runs the test profile

1. Install Nextflow ≥ 24.04.2 and a container engine (Singularity recommended on HPC).
2. Pull the pipeline: `nextflow pull sanjaysgk/ipg`.
3. Run the chr22 test: `nextflow run sanjaysgk/ipg -profile test,singularity --outdir results-test/`.
4. Verify by looking at `results-test/multiqc/multiqc_report.html` and `results-test/<sample>/Database_Construction/<sample>_cryptic.fasta`.

### Workflow 2 — production run on Monash M3

1. Build samplesheet from real FASTQs in `/fs04/scratch2/xy86/...`.
2. Place reference data per the [Reference genome parameters](/api-reference/parameters/reference-genome) page.
3. Submit via `pixi run nextflow run sanjaysgk/ipg -profile monash,singularity --input samplesheet.csv --outdir /fs04/scratch2/xy86/.../results -bg` from inside an `smux` session (so the head process survives logout).
4. Monitor SLURM jobs with `squeue -A xy86`.
5. Fetch the cryptic FASTAs from `results/<sample>/Database_Construction/`.

### Workflow 3 — adapt for a non-Monash HPC

1. Copy `conf/monash.config` to a new `conf/<myhpc>.config`.
2. Update `process.executor`, `process.queue`, `process.account`, container cache path.
3. Register the new profile in `nextflow.config` → `profiles { ... }`.
4. Submit a PR to `sanjaysgk/ipg` so other users at your institution benefit.

## Integration

- **Pipeline source:** https://github.com/sanjaysgk/ipg
- **Original C tools (kescull):** https://github.com/sanjaysgk/immunopeptidogenomics
- **Documentation source:** https://github.com/sanjaysgk/ipg-docs
- **nf-core conventions:** https://nf-co.re

## Context

The IPG pipeline is a faithful nf-core port of the 31-step bash pipeline described in Scull et al. 2021. It groups steps into 6 nf-core subworkflows, wraps 5 custom kescull C tools as local nf-core modules (`curate_vcf`, `revert_headers`, `alt_liftover`, `triple_translate`, `squish`), and uses standard nf-core modules for everything else (STAR, RSeQC, StringTie, gffcompare, GATK4, gffread, MultiQC, FastQC).

The current `ipg` pipeline covers **database construction only** (steps 1–31). The MS-search and biological-classification half of the immunopeptidogenomics workflow (`origins`, `db_compare`, `msDot`, `filter_FPKM`) is the planned `ipg-origins` sister pipeline and is not yet ported.

The pipeline is academic research software intended for citation; agents should never add AI co-authorship to commits or PRs.
