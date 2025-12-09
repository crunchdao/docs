---
description: >-
  Can you design algorithms that identify genes driving obesity and metabolic
  disease?
cover: ../../../.gitbook/assets/banner.webp
coverY: 0
---

# Obesity ML Competition: Tackling Metabolic Diseases

You will get to work with cutting-edge biological data collected in partnership with the [**Eric and Wendy Schmidt Center at the Broad Institute**](https://www.broadinstitute.org/), the [**Broad Diabetes Initiative**](https://www.broadinstitute.org/diabetes/diabetes-genetics-initiative), [**Massachusetts General Hospital**](https://www.massgeneral.org/), and [**Beth Israel Deaconess Medical Center**](https://www.bidmc.org/). Your algorithms could directly guide biological discoveries in obesity!

## Quick TL;DR

* **The Goal**: Identify genetic "switches" that can trick human cells into burning fat instead of storing it.
* **The Data Science**: You will be given Single-Cell RNA sequencing (scRNA-seq) data from cells where specific genes have been knocked out (turned off) using CRISPR/Cas9.
* **The Challenge**: Build a model to predict how a cell’s behavior and development change when a new, unseen gene is turned off.

## Introduction

### The Biology: Storing Energy vs. Burning Energy

Not all body fat is created equal. Our bodies primarily rely on two types of fat cells (adipocytes) to manage energy:

1. **White Fat**: The "storage" cells. They hold onto excess energy from food. When we store too much, it leads to obesity and metabolic diseases like Type 2 Diabetes.
2. **Brown Fat**: The "furnace" cells. Instead of storing energy, they burn sugar and fat to generate heat (thermogenesis).

Most current obesity drugs work by making people feel less hungry. However, scientists are exploring a different approach: **What if we could convince the body to produce more brown fat, or trigger white fat to start burning energy?**

### The Experiment: Flipping Genetic Switches

To find the biological "switches" that control these processes, researchers are conducting massive experiments. They harvest human fat-cell precursors and use CRISPR/Cas9 technology to turn off specific genes, one by one. They then observe:

* Does the cell still become a fat cell?
* Does it act like a white fat cell (storage) or a brown fat cell (burning)?
* How does the cell's internal machinery (gene expression) change?

### Why cannot we physically test every single gene in the human genome?

There are 20,000 genes in our genomes! It would take too much time and money. We need Machine Learning to fill in the gaps. By training on the existing experimental data, your model will predict the biological outcome of turning off genes that haven't been tested yet.

### Why this Matters?

Obesity affects over 890 million people worldwide and is a primary driver of cardiovascular disease and cancer. While weight-loss drugs exist, they don't work for everyone and often carry side effects.

To develop better therapies, we need to understand the fundamental code of metabolism:

* Which genes tell fat cells how to develop?
* Which genes help turn white fat cells into brown/heat-producing fat cells?
* Which genes change how cells store or burn fat?

If your model can successfully simulate how perturbing genes affect fat cells, it will allow researchers to screen thousands of potential drug targets purely computationally. This could dramatically accelerate the discovery of medicines that restore metabolic balance and fight disease.

### The Challenge

In this competition, you are working with single-cell RNA sequencing data, which indicates how much each gene is expressed in each single cell. Scientists have "knocked out" (deleted) different genes in fat-cell precursors and measured how the cells changed.

Your job is to **build a model that predicts what happens when scientists turn off new genes that appear in the test set**.

You must predict two specific outcomes for these unseen gene knockouts:

#### 1. The "Internal State" (Gene Expression Profiles)

You must predict the gene expression profile of single cells after the target gene is knocked out. This is a high-dimensional vector representing the activity levels of thousands of genes within the cell.

#### 2. The "Cell Identity" (Cell Type differentiation)

Normally, these precursor cells differentiate into specific types of fat cells. You must estimate the resulting proportions of four distinct cell states:

* **Pre-adipocyte**: Early-stage cells that haven't differentiated yet.
* **Adipocyte**: Mature fat cells (the standard white fat).
* **Lipogenic**: Specialized fat-producing cells.
* **Other**: Cells that followed a different developmental path.

{% hint style="success" %}
Explore the [full specifications](full-specifications.md) for in-depth details.
{% endhint %}

## Phases

The challenge is broken down into three Crunches.

### Crunch 1 – Predicting the effect of held-out single-gene perturbations

From Dec 8 to May 1, Crunchers will build a model to predict the single-cell transcriptomic response to unseen single-gene perturbations.

### Crunch 2 – Predicting the effect of held-out double-gene perturbations

From Feb 28 to May 1, Crunchers will build a model to predict the single-cell transcriptomic response to unseen double-gene perturbations. It will be very similar to the Crunch 1, so we highly encorage participation to both!

{% hint style="success" %}
More details about this challenge will be announced soon!
{% endhint %}

### Crunch 3 – Identifying combinatorial perturbations to drive white and brown adipocyte differentiation

Crunchers will predict combinatorial perturbations to drive adipocyte differentiation, and The Eric and Wendy Schmidt Center will test these perturbations directly in the lab!

{% hint style="success" %}
More details about this challenge will be announced soon!
{% endhint %}

## Timeline

* **December 2025:**
  * Beginning of Crunch 1
* **February 2026:**
  * Closing of Crunch 1
  * Beginning of Crunch 2
* **March 2026:**
  * Closing of Crunch 2
  * Beginning of Crunch 3
* **April 2026:**
  * Closing of Crunch 3

## Evaluation Criteria

For each Crunch, participants must submit predictions in `h5ad` format along with matrices containing the predicted cell state proportions for each perturbation.

Outputs will be evaluated using:

* **Pearson Delta** (Crunch 1 & 2)
* **Maximum Mean Discrepancy or MMD** (Crunch 1 & 2)
* **L1-Distance** (Crunch 1 & 2)

To avoid overfitting, Crunch will only publish the public leaderboard once each week.

## Prizes

Crunch 1 and Crunch 2 will use multiple metrics to rank participants. Your final prize will be the sum of the prizes for each metric that you or [your team](../../../crunch-hub/teams.md) ranks in.

All prizes are in [USDC](https://fr.usdc.com/), a cryptocurrency with the same value as the US dollar.

### Crunch 1

| Winner's Rank | Pearson Delta | MMD   | L1 Distance |
| ------------- | ------------- | ----- | ----------- |
| 1st place     | 1,400         | 1,400 | 1,400       |
| 2nd place     | 800           | 800   | 800         |
| 3rd place     | 480           | 480   | 480         |
| 4th place     | 320           | 320   | 320         |
| 5th place     | 240           | 240   | 240         |
| 6th place     | 200           | 200   | 200         |
| 7th place     | 200           | 200   | 200         |
| 8th place     | 160           | 160   | 160         |
| 9th place     | 120           | 120   | 120         |
| 10th place    | 80            | 80    | 80          |
| Total         | 4,000         | 4,000 | 4,000       |

### Crunch 2

| Winner's Rank | Pearson Delta | MMD   | L1 Distance |
| ------------- | ------------- | ----- | ----------- |
| 1st place     | 1,400         | 1,400 | 1,400       |
| 2nd place     | 800           | 800   | 800         |
| 3rd place     | 480           | 480   | 480         |
| 4th place     | 320           | 320   | 320         |
| 5th place     | 240           | 240   | 240         |
| 6th place     | 200           | 200   | 200         |
| 7th place     | 200           | 200   | 200         |
| 8th place     | 160           | 160   | 160         |
| 9th place     | 120           | 120   | 120         |
| 10th place    | 80            | 80    | 80          |
| Total         | 4,000         | 4,000 | 4,000       |

### Crunch 3

| Winner's Rank | To be determined |
| ------------- | ---------------- |
| 1st place     | 7,000            |
| 2nd place     | 6,500            |
| 3rd place     | 5,000            |
| 4th place     | 3,000            |
| 5th place     | 1,000            |
| 6th place     | 900              |
| 7th place     | 800              |
| 8th place     | 700              |
| 9th place     | 600              |
| 10th place    | 500              |
| Total         | 26,000           |

## External Resources

Crunchers are encouraged to use publicly available external resources, including gene perturbation datasets and pre-trained models, as long as they are properly credited.

## References

Below are a few references meant to provide more background and some of the approaches researchers are applying in the fields relevant to these Crunches. This is not meant to be an exhaustive list and many important works are not listed here.

<details>

<summary>Single cell transcriptomic adipocytes datasets</summary>

* [Dissecting the impact of transcription factor dose on cell reprogramming heterogeneity using scTF-seq](https://www.nature.com/articles/s41588-025-02343-7)
* [A single-cell atlas of human and mouse white adipose tissue](https://www.nature.com/articles/s41586-022-04518-2)
* [Human subcutaneous and visceral adipocyte atlases uncover classical and nonclassical adipocytes and depot-specific patterns](https://www.nature.com/articles/s41588-024-02048-3)
* [Adipose tissue retains an epigenetic memory of obesity after weight loss.](https://www.nature.com/articles/s41586-024-08165-7)
* [Unveiling adipose populations linked to metabolic health in obesity](https://www.cell.com/cell-metabolism/fulltext/S1550-4131\(24\)00452-2?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS1550413124004522%3Fshowall%3Dtrue)
* [Spatial mapping reveals human adipocyte subpopulations with distinct sensitivities to insulin](https://www.cell.com/cell-metabolism/fulltext/S1550-4131\(21\)00363-6?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS1550413121003636%3Fshowall%3Dtrue)
* [snRNA-seq reveals a subpopulation of adipocytes that regulates thermogenesis](https://www.nature.com/articles/s41586-020-2856-x)
* [Wnt signaling preserves progenitor cell multipotency during adipose tissue development](https://www.nature.com/articles/s42255-023-00813-y)
* [Adipogenic and SWAT cells separate from a common progenitor in human brown and white adipose depots](https://www.nature.com/articles/s42255-023-00820-z)
* [Single-Nucleus Analysis of Human White Adipose Tissue Reveals Adipocyte Subsets with Distinct Metabolic Profiles](https://www.biorxiv.org/content/10.1101/2025.09.14.673351v1)
* [Mapping the transcriptional landscape of human white and brown adipogenesis using single-nuclei RNA-seq](https://www.sciencedirect.com/science/article/pii/S2212877823000807?via%3Dihub)

</details>

<details>

<summary>CRISPR/Cas9 genetic perturbation screens</summary>

* [Mapping information-rich genotype-phenotype landscapes with genome-scale Perturb-seq](https://www.sciencedirect.com/science/article/pii/S0092867422005979?via%3Dihub)
* [Exploring genetic interaction manifolds constructed from rich single-cell phenotypes](https://www.science.org/doi/10.1126/science.aax4438)
* [X-Atlas/Orion: Genome-wide Perturb-seq Datasets via a Scalable Fix-Cryopreserve Platform for Training Dose-Dependent Biological Foundation Models](https://www.biorxiv.org/content/10.1101/2025.06.11.659105v1.full.pdf)

</details>

<details>

<summary>Modeling perturbations and causality</summary>

* [Machine learning for perturbational single-cell omics](https://www.sciencedirect.com/science/article/pii/S2405471221002027)
* [Elements of Causal Inference: Foundations and Learning Algorithms](https://library.oapen.org/handle/20.500.12657/26040)
* [Causal Structure and Representation Learning with Biomedical Applications](https://arxiv.org/abs/2511.04790)
* [scPerturb: Information Resource for Harmonized Single-Cell Perturbation Data](https://pubmed.ncbi.nlm.nih.gov/38279009/)
* [GeneDisco: A Benchmark for Experimental Design in Drug Discovery](https://arxiv.org/abs/2110.11875)
* [Systema: a framework for evaluating genetic perturbation response prediction beyond systematic variation](https://www.nature.com/articles/s41587-025-02777-8)
* [Deep-learning-based gene perturbation effect prediction does not yet outperform simple linear baselines](https://www.nature.com/articles/s41592-025-02772-6)
* [MORPH Predicts the Single-Cell Outcome of Genetic Perturbations Across Conditions and Data Modalities](https://pmc.ncbi.nlm.nih.gov/articles/PMC12236822/)
* [Learning Genetic Perturbation Effects with Variational Causal Inference](https://www.biorxiv.org/content/10.1101/2025.06.05.657988.abstract?__cf_chl_tk=OYKEAztwqhTV5wR88BPPWiPAPeKXBcS5JGYnXetMlkI-1764097427-1.0.1.1-vxNBhPFfL036W6lSNAyhsc2qWS4ZOJGP.EQamGrNcco)
* [Squidiff: predicting cellular development and responses to perturbations using a diffusion model](https://www.nature.com/articles/s41592-025-02877-y)
* [Predicting cellular responses to perturbation across diverse contexts with State](https://www.biorxiv.org/content/10.1101/2025.06.26.661135v2)
* [TxPert: Leveraging Biochemical Relationships for Out-of-Distribution Transcriptomic Perturbation Prediction](https://arxiv.org/abs/2505.14919)
* [GEARS: Predicting transcriptional outcomes of novel multi-gene perturbations](https://www.nature.com/articles/s41587-023-01905-6)
* [Learning Causal Representations of Single Cells via Sparse Mechanism Shift Modeling](https://arxiv.org/abs/2211.03553)
* [Predicting cellular responses to complex perturbations in high-throughput screens](https://pubmed.ncbi.nlm.nih.gov/37154091/)
* [PerturbNet predicts single-cell responses to unseen chemical and genetic perturbations](https://pubmed.ncbi.nlm.nih.gov/40640612/)
* [Predicting Cellular Responses to Novel Drug Perturbations at a Single-Cell Resolution](https://arxiv.org/abs/2204.13545)
* [Active Learning for Optimal Intervention Design in Causal Models](https://www.nature.com/articles/s42256-023-00719-0)
* [Control of cell state transitions](https://www.nature.com/articles/s41586-022-05194-y)

</details>

{% hint style="info" %}
Reading these articles is not necessary to complete the challenge, but we believe these can be a helpful resource.
{% endhint %}
