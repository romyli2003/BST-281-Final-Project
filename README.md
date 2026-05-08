# BST-281-Final-Project

## Module 1 Handoff Overview

This repository contains the Module 1 handoff files for our BST 281 final project on single-nucleus RNA-seq analysis of mouse models of Alzheimer's disease. Module 1 focused on:

- quality control
- normalization and integration
- broad clustering and cell-type annotation
- exporting downstream-ready objects and summary tables for Modules 2 to 4

The goal of this handoff is to provide a shared reference map that the rest of the team can use for microglia-focused differential expression, compositional analysis, and glial trajectory/network analysis.

## Dataset

Source dataset: **GSE140510**  
Samples included: **12 total**, spanning 4 conditions with 3 biological replicates each:

- WT
- Trem2_KO
- WT_5XFAD
- Trem2_KO_5XFAD

## Processing summary

Starting from the GEO raw 10x-format matrices, I:

1. imported all 12 samples into Seurat
2. computed QC metrics and applied initial filtering
3. normalized and integrated samples using an **RPCA reference-based Seurat workflow**
4. performed clustering and generated a shared UMAP/PCA embedding
5. assigned **broad cell-type labels** at the cluster level
6. exported subset objects and count tables needed for downstream modules

### QC thresholds used

Initial filtering was based on:

- `nFeature_RNA >= 400`
- `nFeature_RNA <= 6000`
- `nCount_RNA <= 20000`
- `percent.mt <= 10`

### Integration / clustering notes

- Integration method: **Seurat RPCA reference-based integration**
- Reference samples used:
  - `WT2`
  - `Trem2_KO2`
  - `WT_5XFAD1`
  - `Trem2_KO_5XFAD2`
- Clustering resolution used for the handoff: **0.3**

## Annotation notes

Broad cluster annotations were reviewed manually using:

- cluster marker tables
- canonical marker dot plots
- known broad brain cell-type markers

The broad cell types used in the handoff are:

- Astrocyte
- Endothelial
- Fibroblast_Stromal
- Microglia
- Neuron
- Oligodendrocyte
- OPC
- Pericyte_VSMC

A low-quality cluster was identified and excluded from the main handoff object:

- **cluster 12 -> Low_quality**

## Main handoff files

### Core Seurat objects

- **`module1_integrated_handoff_no_lowq.rds`**  
  Main downstream handoff object. Annotated integrated Seurat object with the low-quality cluster removed.

- **`module1_integrated_annotated_full.rds`**  
  Full annotated integrated object before removing the low-quality cluster.

### Cell-level metadata

- **`cell_metadata_handoff_no_lowq.csv`**  
  Per-cell metadata for the cleaned handoff object.

- **`cell_metadata_annotated_full.csv`**  
  Per-cell metadata for the full annotated object.

### Annotation support files

- **`cluster_annotation_key_reviewed.csv`**  
  Reviewed cluster-to-broad-cell-type mapping used in the handoff.

- **`cluster_markers_all.csv`**  
  Full marker table from `FindAllMarkers()`.

- **`cluster_top10_markers.csv`**  
  Top 10 markers per cluster for quick inspection.

- **`canonical_marker_dotplot_data.csv`**  
  Dot-plot expression summary for canonical marker genes used during annotation.

## Downstream module files

### For Module 2: Microglia differential expression / enrichment

- **`microglia_subset.rds`**  
  Seurat object containing only annotated microglia.

- **`microglia_metadata.csv`**  
  Metadata for microglial cells.

- **`microglia_barcodes.csv`**  
  Barcode list for microglial cells.

### For Module 3: Composition / differential abundance analysis

- **`sample_by_celltype_counts_long.csv`**  
  Observed sample-by-cell-type counts with within-sample proportions.

- **`sample_by_celltype_counts_wide.csv`**  
  Wide-format sample-by-cell-type count matrix.

- **`sample_by_cluster_counts_long.csv`**  
  Observed sample-by-cluster counts with within-sample proportions.

- **`sample_by_cluster_counts_wide.csv`**  
  Wide-format sample-by-cluster count matrix.

- **`sample_info_handoff.csv`**  
  One row per sample with condition, replicate, GSM accession, Trem2 status, and AD status.

### For Module 4: Glial trajectory / network analysis

- **`glia_subset.rds`**  
  Seurat object containing glial populations only.

- **`glia_metadata.csv`**  
  Metadata for glial cells.

- **`glia_barcodes.csv`**  
  Barcode list for all glial cells.

Optional barcode files for specific glial subtypes are also included:

- `astrocyte_barcodes.csv`
- `oligodendrocyte_barcodes.csv`
- `opc_barcodes.csv`

## Recommended starting points by module

- **Module 2** should start from `microglia_subset.rds`
- **Module 3** should start from `sample_by_celltype_counts_long.csv` and `sample_by_cluster_counts_long.csv`
- **Module 4** should start from `glia_subset.rds`

## Important notes / caveats

- This handoff is intended to support **broad cell-type level** downstream work.
- Broad annotations were manually reviewed, but they are not meant to represent final fine-grained state annotation.
- One low-quality cluster was removed from the main handoff object to avoid contaminating downstream analyses.
- Count tables were generated from **observed sample-condition combinations only**, not from all possible combinations.

## Reproducibility

Intermediate checkpoints from earlier processing steps were also saved during analysis, but the files in this handoff tarball are the final downstream-ready outputs.

## Contact

Prepared by **Romy Li** for Module 1 handoff.
