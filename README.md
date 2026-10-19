# Comparative Analysis of Polyhydroxyalkanoate Synthases Across Four Enzyme Classes

**Biotechnology Research Internship — Week 3: Bioinformatics Data Analysis and Interpretation**
**Rabi Narayan Patra** | 17 October 2026

A complete bioinformatics pipeline on a real protein dataset — acquisition, cleaning, alignment,
**validation**, phylogeny, conservation analysis and motif discovery — interpreted for a biotechnological
product question.

## The dataset

**18 PHA synthase (PhaC) sequences** spanning the four recognised enzyme classes plus an archaeal outgroup,
retrieved through the **UniProt REST API** so the exact set is reproducible.

| Class | Subunits | Substrate range | Examples |
|---|---|---|---|
| I | single, ~600 aa | scl, C3–C5 | *Cupriavidus*, *Halomonas* |
| II | single, ~560 aa | mcl, C6–C14 | *Pseudomonas* |
| III | PhaC + PhaE | scl | *Allochromatium*, *Synechocystis* |
| IV | PhaC + PhaR | scl | *Bacillus*, *Priestia* |

## Results

**Data cleaning** removed 2 of 18 sequences against a rule fixed before inspection (length < 80% of the
sequence's own class median). Both removals are defensible: the *Burkholderia* entry at 231 aa is 39% of its
class median and is plainly a fragment.

**Alignment validation** was the step that mattered most. An alignment is a hypothesis, and the test applied
here is simple: *a correct alignment must place same-class sequences closer together than different-class
sequences.* The first method implemented — a progressive alignment following a guide tree — **failed** it, with
distances correlating **−0.16** with a direct pairwise estimator and the taxonomic signal **inverted**
(within-class 0.042 vs between-class 0.063). It was replaced with a reference-guided alignment, which passes at
**r = 0.942** with separations of **+0.237** and **+0.225**.

**Phylogeny and robustness.** Neighbour joining with a 100-replicate site bootstrap recovers **all five groups
as monophyletic clades in 100% of replicates**. The deep nodes *between* classes have low support — expected
for an ancient, highly divergent family — and both facts are reported.

**Identity structure.** Within-class mean identity **0.460** against **0.224** between classes; the heatmap
ordered by the phylogeny shows clean class blocks.

**Feature discovery.** 15 invariant columns and 22 at ≥90% identity, including an **invariant catalytic
cysteine** (alignment column 318, consensus context L-L-G-Y-C-M-G-G-T) — conserved across bacteria *and*
archaea.

**The class-specific lipase-box motif** — the headline finding:

| Class | Motif variant |
|---|---|
| I | **G-[FY]-C-[VI]-G-G** |
| II | **G-A-C-A-G-G** |
| III | **G-[IV]-C-Q-G-G** |
| IV | **G-Y-C-M-G-G** |

The scan was run **blind** — the pattern alone was searched for, with no expectation about which variant would
appear — and the variants segregate by class.

## Why it matters for product development

Because the lipase-box window shapes which hydroxyacyl-CoA chain lengths the enzyme accepts, the motif is a
**diagnostic fingerprint for substrate range**: an uncharacterised synthase can be assigned a probable class,
and a probable product spectrum, from **seven residues of sequence**. For a programme screening a metagenomic
library, that converts a wet-lab assay per candidate into a sequence filter applied to thousands at once.

The conservation result points the opposite way and is a **caution**: the catalytic cysteine is invariant
across a billion years of divergence, which is what you would expect if any substitution abolished activity —
so it is *not* a sensible engineering target. The residues that differ between classes are the ones worth
changing. The analysis thus separates what to leave alone from what to engineer.

## Repository contents

```
├── README.md
├── Bioinformatics_Analysis_and_Interpretation_Biotech.docx   # the report (16 pages, ~3,900 words)
├── Bioinformatics_Analysis_and_Interpretation_Biotech.pdf
├── references.md                                            # the 16 sources
└── figures/                                                 # all six figures
```

**In the v1.0 release bundle:** the report in both formats, all six figures, the three analysis scripts, the
curated dataset, and every intermediate result file.

## Reproducing it

```bash
python3 scripts/01_fetch_sequences.py   # UniProt query + curation
python3 scripts/02_analysis.py          # cleaning, alignment, validation, tree, motifs
python3 scripts/03_figures.py           # all six figures
python3 scripts/04_build_report.py      # assembles the report
```

## A note on the file format

The task specifies PDF; the portal's upload box accepts **Word only**. Both are provided.

## Author

**Rabi Narayan Patra** — Biotechnology Research Intern, 2026.
GitHub: [@rabinarayanpatra350-biotech](https://github.com/rabinarayanpatra350-biotech)
