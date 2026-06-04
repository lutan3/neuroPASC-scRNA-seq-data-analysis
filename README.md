# neuroPASC-scRNA-seq-data-analysis
This repository contains R scripts for processing and analyzing single-cell RNA sequencing (scRNA-seq) data derived from brain CD45+ immune cells collected from a neuroPASC mouse model.

## Table of Contents
- [Overview](#overview)
- [Requirements](#requirements)
- [Installation](#installation)
- [Scripts](#scripts)
- [Usage](#usage)

## Overview

These scripts support the analysis of scRNA-seq data from mouse brain immune cells and the investigation of intercellular communication networks. The repository includes workflows for:

- Quality control, filtering, and extraction of immune cells
- Ambient RNA decontamination and cell clustering analysis
- Microglial subclustering, mitochondrial signature analysis, and differential gene expression analysis
- Trajectory analysis of microglial subclusters
- Brain-associated macrophage (BAM) subtype characterization and differential gene expression analysis
- Monocyte subtype identification, characterization, and differential gene expression analysis
- Neutrophil subtype identification, characterization, and differential gene expression analysis
- T-cell differential gene expression analysis
- B-cell differential gene expression analysis
- CellChat-based analysis of intercellular communication
- CellChat analysis incorporating microglial subclusters

## Requirements

### R Environment
- **R version:** 4.5.0 or higher
- **Platform:** Compatible with macOS, Linux, and Windows

### R Packages

```r
# Core packages
Seurat	5.3.1	CRAN
SeuratObject	5.2.0	CRAN
SingleCellExperiment	1.30.1	Bioconductor
monocle3	1.4.26	GitHub (cole-trapnell-lab/monocle3, commit 4f4239a)
CellChat	2.2.0	GitHub (jinworks/CellChat, commit 346fb61)
tidyverse	2.0.0	CRAN
dplyr	1.1.4	CRAN
Matrix	1.7-3	CRAN
edgeR	4.6.3	Bioconductor

# Additional packages
ggplot2	4.0.1
cowplot	1.2.0
ggpubr	0.6.1
pheatmap	1.0.13
ggforce	0.5.0
ggcorrplot	0.1.4.1
scales	1.4.0
reshape2	1.4.5
umap	0.2.10.0
clusterProfiler	4.16.0
org.Mm.eg.db	3.21.0
AnnotationDbi	1.70.0
Statistical analysis
Package	Version
rstatix	0.7.2
readxl	1.4.5
purrr	1.2.2
RCurl	1.98-1.17
Matrix.utils	0.9.7

# Additional dependencies
Several packages are required indirectly by Seurat, monocle3, CellChat, and Bioconductor workflows, including:
sctransform 0.4.2
igraph 2.2.1
patchwork 1.3.2
limma 3.64.1
BiocParallel 1.42.1
SummarizedExperiment 1.38.1
GenomicRanges 1.60.0
IRanges 2.42.0
S4Vectors 0.46.0
GenomeInfoDb 1.44.1
These dependencies are automatically installed when the corresponding packages are installed through CRAN, Bioconductor, or GitHub.
```

## Installation

```r
# CRAN packages
install.packages(c(
  "Seurat",
  "tidyverse",
  "cowplot",
  "ggpubr",
  "pheatmap",
  "ggforce",
  "ggcorrplot",
  "scales",
  "reshape2",
  "umap",
  "readxl",
  "purrr",
  "RCurl"
))

# Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(c(
  "SingleCellExperiment",
  "edgeR",
  "clusterProfiler",
  "org.Mm.eg.db"
))

# GitHub packages
remotes::install_github("cole-trapnell-lab/monocle3")
remotes::install_github("jinworks/CellChat")
```

## Scripts

#### 0. Demo data
We provided [demo data] (https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE311097) to run R scripts.

#### 1. `Quality control, filtering, and extraction of immune cells.R`
Performs quality control filtering to remove low-quality cells and extracts immune cells expressing **CD45 (Ptprc)** or **CD11b (Itgam)**.
- **Input:** Raw single-cell RNA-seq Seurat object.
- **Output:** A Seurat object containing filtered CD45⁺ or CD11b⁺ immune cells after removal of low-quality cells.

#### 2. `Ambient RNA decontamination and cell clustering analysis.R`
Performs ambient RNA decontamination using DecontX, removes highly contaminated cells and clusters, conducts unsupervised clustering of the cleaned single-cell dataset, and perform cell type annotation.
- **Input:** Filtered CD45⁺ or CD11b⁺ Seurat object generated from Script 1.
- **Output:** A cleaned Seurat object containing cell cluster assignments and cell type annotation.

#### 3. `Microglial subclustering, mitochondrial signatures, and differential expression analysis.R`
Performs microglia-specific reclustering and identifies microglial subpopulations. The script also conducts differential gene expression analyses across microglial subclusters and infection time points, assesses mitochondrial gene regulation, and quantifies transcriptional dysregulation dynamics during disease progression.
- **Input:** Annotated and decontaminated Seurat object generated from Script 2.
- **Output:**
Reclustered and annotated microglial Seurat object
Cluster-specific marker gene tables
Differentially expressed gene tables for microglial subclusters and infection time points
Module score analyses for proliferation, activation, DAM, cytoskeletal, RNA-processing, protein-turnover, and mitochondrial signatures
Mitochondrial gene enrichment and regulation analyses
UMAP visualizations, heatmaps, dot plots, violin plots, correlation analyses, and transcriptional dysregulation trajectories

#### 4. `Trajectory analysis of microglial subclusters.R`
Performs pseudotime trajectory analysis of microglial subpopulations using Monocle3 to infer cellular state transitions and progression dynamics.
- **Input:** Annotated microglial Seurat object generated from Script 3.
- **Output:** UMAP trajectory visualizations colored by pseudotime and pseudotime distribution plots across samples and microglial subclusters.

#### 5. `BAM subtype characterization and differential gene expression analysis.R`
Performs comprehensive characterization of border-associated macrophage (BAM) subpopulations and their transcriptional responses across infection conditions. 
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** BAM subtype marker genes and differential gene expression results across time points.

#### 6. `Monocytes subtype identification, characterization, and differential gene expression analysis.R`
Performs comprehensive characterization of monocyte subpopulations and transcriptional responses across time points.
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** Monocyte subtype marker genes and differential gene expression results across time points.

#### 7. `Neutrophil subtype identification, characterization, and differential gene expression analysis.R`
Performs comprehensive characterization of neutrophil subpopulations and transcriptional responses across time points.
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** Neutrophil subcluster gene module scoring results and differential gene expression results across time points.

#### 8. `T cell differential gene expression analysis.R`
Performs single-cell transcriptomic analysis of T cell populations across infection conditions. 
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** Differential gene expression results across infection time points (6, 30, 100 dpi vs mock).

#### 9. `B cell differential gene expression analysis.R`
Performs single-cell transcriptomic analysis of B cell populations across infection conditions. 
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** Differential gene expression results across infection time points (6, 30, 100 dpi vs mock).

#### 10. `CellChat analysis of intercellular communication.R`
Performs comprehensive inference of cell–cell communication networks across immune cell populations using scRNA-seq data.
- **Input:** Annotated single-cell Seurat object generated from Script 2.
- **Output:** Ligand–receptor interaction tables for selected sender–receiver pairs and visualization outputs including heatmaps of communication intensity, differential interaction bubble plots, and pairwise condition comparisons of signaling networks.

#### 11. `CellChat analysis incorporating microglial subclusters.R`
Performs comprehensive inference of cell–cell communication networks among microglial subclusters and immune cell populations.
- **Input:** Annotated single-cell Seurat object generated from Script 2 and annotated microglial Seurat object from Script 3.
- **Output:** Ligand–receptor interaction tables for selected sender–receiver pairs and visualization outputs including heatmaps of communication intensity, differential interaction bubble plots, and pairwise condition comparisons of signaling networks.

## Usage

### Basic Workflow

1. **Run R analysis scripts in order:**
   ```r
   source("Quality control, filtering, and extraction of immune cells.R")
   source("Ambient RNA decontamination and cell clustering analysis.R")
   source("Microglial subclustering, mitochondrial signature analysis, and differential gene expression analysis.R")
   # ... continue with other scripts as needed
   ```

2. **Customize parameters** within each script according to your data and analysis needs.

## Session Information

The scripts were developed and tested in the following environment:

```r
R version 4.5.0 (2025-04-11)
Platform: aarch64-apple-darwin20
Running under: macOS 26.0.1
```

## Contact

For questions or issues, please open an issue in this repository or contact Lu Tan (lu-tan@uiowa.edu).

---

**Note:** Ensure input file paths are correctly specified in the scripts before running.
