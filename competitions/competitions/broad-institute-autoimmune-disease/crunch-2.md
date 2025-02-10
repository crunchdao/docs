---
cover: ../../../.gitbook/assets/banner (1).png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Crunch 2 – Nov 18 to Mar 21 – Predicting Unseen Genes

## Overview

In **Crunch 2**, your task is to predict the expression levels of genes that were **not measured** in a spatial transcriptomics dataset. You will use both spatial data and single-cell RNA sequencing (scRNA-Seq) data from similar colon tissue samples to make these predictions.

### **X (Inputs Data)**

* **Spatial Data**: The `.zarr` data provided in [**Crunch 1**](crunch-1.md#linking-the-h-and-e-image-to-spatial-transcriptomics).
* **scRNA-Seq Data**: The `Crunch2_scRNAseq.h5ad` file contains gene expression data for **18,615 protein-coding genes**, including the **460 genes** in the Spatial Data object.

### **Y (Targets)**

* **Gene Expression Predictions**: The expression levels of **2,000 genes**.

<div data-full-width="false"><figure><img src="../../../.gitbook/assets/Capture d’écran 2024-11-18 à 19.47.47.png" alt=""><figcaption><p>Example of 2,000 genes expressions with random values.</p></figcaption></figure></div>

## Evaluation Phases

In Crunch 2, you will have the opportunity to evaluate your model’s predictive performance on a validation dataset, before submission of your test dataset predictions.

There will be checkpoints every:

* **Friday** — to get your scores before the weekend
* **Monday** — to see how your weekend work stacks up

The final submission must be submitted by **March 21th (Eastern Time 17:59)**.

## The single-cell transcriptomic datasets

Colon tissue samples similar to those profiled by Xenium spatial transcriptomics.

These datasets contain single-cell gene expression data for **18,615 protein-coding genes**, including the 460 genes in the Spatial Data.

### **Datasets Included**

We provide datasets (atlases) from multiple studies to represent all the cell types that are found in the colon tissue.

* [**UC Dataset**](https://pubmed.ncbi.nlm.nih.gov/31348891/): An atlas of ulcerative colitis patients, including inflamed, non-inflamed, and healthy colon tissue.
* [**ENS Dataset**](https://pubmed.ncbi.nlm.nih.gov/32888429/): An atlas of the enteric nervous system, including glial cells and neurons innervating the colon.
* [**Muscle Dataset**](https://pubmed.ncbi.nlm.nih.gov/37206377/): An atlas of the colon muscle layer.

### **Data Format**

Provided as an [AnnData object](https://anndata.readthedocs.io/en/latest/) stored in an h5ad file: `Crunch2_scRNAseq.h5ad`.

#### **Cell Metadata `scRNA-Seq.obs`**

* **Cell Type**: `scRNA-Seq.obs["annotation"]`
* **Study**: `scRNA-Seq.obs["study"]`
* **Individual**: `scRNA-Seq.obs["individual"]`
* **Disease Status**: `scRNA-Seq.obs["status"]`

<figure><img src="../../../.gitbook/assets/ScRNA-Seq obs (1).png" alt="scRNA-Seq Observations (scRNAseq.obs)"><figcaption></figcaption></figure>

#### **Expression Data`scRNA-Seq.X`** — `log1p-normalized` counts

Original raw counts per cell are divided by the sum of counts per cell, multiplied by **10,000**, and then `log1p`-transformed.

<div data-full-width="false"><figure><img src="../../../.gitbook/assets/ScRNA-Seq X (2).png" alt="Compressed Sparse Row (CSR) Matrix of scRNAseq.X as DataFrame"><figcaption></figcaption></figure></div>

This representation displays the `scRNAseq.X` matrix in **DataFrame** format to clarify the structure of the CSR matrix.

The columns in the DataFrame are as follows:

* Row: the row index corresponding to the observation index, accessible via `scRNAseq.obs`
* Column: the column index corresponding to the gene index, accessible via `scRNAseq.var`
* Value: normalized, log-transformed gene expression counts

#### **Raw Gene Counts** — Available in `scRNA-Seq.layers["counts"]`

## Expected Output

The output must be provided as a DataFrame with the following structure:

**Index**

* Contains the `cell_id` values corresponding to the validation and test groups expected in the SpatialData (`.zarr` file provided by the `infer` function).

**Columns**

* Contains **2,000 genes** randomly selected from the **18,615 protein-coding genes** in the `scRNA-Seq` data, including the **20 genes** already measured by Xenium spatial transcriptomics but excluded from the Spatial Data object.
* You can retrieve this list from the `Crunch2_gene_list.csv` file included in the competition dataset.

**Values**

* Gene expression predictions for each cell and gene.
* Predictions must be **log1p-normalized** and rounded to **2 decimal points**.

{% hint style="info" %}
Refer to the [`random-submission.ipynb`](https://github.com/crunchdao/competitions/blob/master/competitions/broad-2/quickstarters/random-submission/random-submission.ipynb) notebook for an example of how to format your submission.
{% endhint %}

## Evaluation

Your predictions are evaluated on the **20 held-out genes** using **Spearman’s rank correlation** for cells with non-zero expression. For cells with zero expression, a separate metric applies. Scores combine predictions across **global and local regions** for a balanced final score.
