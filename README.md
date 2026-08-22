# Tumor-Purity-Relatively-Stable-Genes
5-anchor CPE-based deconvolution identifying genes relatively stable to tumor purity across BRCA, KIRC, and LUSC (TCGA). Includes confounder-checked differential expression, leaky-gene filtering, penetrance tiering, and visualization code accompanying the methods manuscript.
# Tumor-Purity-Relatively-Stable-Genes

A five-anchor CPE-based deconvolution framework for identifying genes whose tumor-versus-normal differential expression remains relatively stable across the full gradient of tumor purity — applied to three histologically and embryologically distinct TCGA cohorts: kidney renal clear cell carcinoma (KIRC), lung squamous cell carcinoma (LUSC), and breast invasive carcinoma (BRCA).

## Overview

Tumor purity — the proportion of malignant cells within a bulk tumor sample — varies considerably even within a single cancer type, and many genes shift in measured expression simply as a function of this variation rather than any real change in tumor biology. This framework uses TCGA's Consensus Purity Estimate (CPE) to stratify tumor samples into five ordinal purity groups (very low, low, moderate, high, very high) and identifies genes whose differential expression against normal tissue holds consistently across that gradient, rather than tracking purity itself.

Each purity group is:
1. Compared directly against normal tissue (Set A, ≥2-fold change, adjusted p < 0.05)
2. Compared against its non-adjacent purity groups, using a threshold scaled to the actual CPE gap between the groups
3. Filtered to remove genes explained by that group's position on the purity gradient
4. Intersected across all five anchors, then passed through a final ambiguous-response ("leaky gene") filter

The result is a conservative, high-confidence gene panel per cohort — genes that are **relatively stable to the change in tumor purity**, not merely differentially expressed in one purity band.

## Results

| Cohort | Upregulated | Downregulated |
|--------|------------:|--------------:|
| KIRC   | 132         | 470           |
| LUSC   | 86          | 303           |
| BRCA   | 99          | 403           |

Recurring, literature-consistent biological themes were recovered across all three cohorts, including cell-cycle/DNA-repair signatures, loss-of-differentiation markers, and an unexpected cross-cohort neuronal/synaptic gene cluster — despite the pipeline's deliberately stringent, purity-scaled filtering.

## Repository Structure
Each script is self-contained and covers data acquisition (TCGAbiolinks), clinical annotation and QC, CPE-based purity stratification, confounder assessment, limma-voom differential expression, purity-independent gene set derivation, penetrance tiering, and visualization (heatmaps, violin plots, UpSet plots).

## Methodology Summary

- **Data**: RNA-seq counts (STAR–Counts) and clinical metadata via `TCGAbiolinks`
- **Purity stratification**: CPE quantiles (q80/q60/q40/q20), applied identically across cohorts
- **Confounder handling**: batch, age, stage, and (for BRCA) PAM50 subtype tested per comparison; batch always included regardless of significance
- **Differential expression**: `edgeR` filtering/TMM normalization + `limma`-`voom`
- **Batch correction for effect-size estimation**: `sva::ComBat`
- **Visualization**: `pheatmap`, `ggplot2`, `UpSetR`

See the manuscript (preprint link to be added) for full methodological detail.

## Requirements

R (≥ 4.2) with the following packages:
`TCGAbiolinks`, `SummarizedExperiment`, `edgeR`, `limma`, `sva`, `tidyverse`, `ggplot2`, `pheatmap`, `UpSetR`, `writexl`

## Usage

Each script can be run independently and end-to-end for its respective cohort, provided the required packages are installed and TCGA data access is available via `TCGAbiolinks`.

## License

See [LICENSE](LICENSE).

## Contact

Murali Dashna Anandan 
email: murali.dashna@hotmail.com
