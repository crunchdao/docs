---
description: Predict the single-cell transcriptomic response to double-gene perturbations.
---

# Crunch 2 – Predicting the effect of held-out double-gene perturbations

## Evaluation Phases

In Crunch 2, you will have the opportunity to evaluate the predictive performance of your model on a validation dataset.

There will be multiple validation checkpoints, with one occurring every Monday at 6:00 p.m. UTC:

* Checkpoint 1 - March 9th
* Checkpoint n - every Monday
* Last checkpoint - April 6th
* Last submission - April 13th
  * Start of the selection period
* End of the selection period - April 17th

{% hint style="info" %}
You can still submit and run your code multiple times onto the platform.

At a checkpoint, all of your (non scored) predictions will be scored. \
Predictions will also be scored at the beginning of the selection period.
{% endhint %}

## Overview

In this Crunch, we will explore how well we can predict the single-cell transcriptomic response to double-gene perturbations that were not provided in the training dataset.

## Dataset

This dataset contains single-gene and pairwise perturbations targeting a curated set of 18 genes. This includes a set of:

* 153 heterotypic perturbations (i.e., Gene A+Gene B)
* 18 homotypic perturbations (i.e., Gene A+Gene A);
* and 18 monogenic perturbations (i.e., Gene A+NC).

Each cell receives either 1 guide RNA (resulting in a single genetic perturbation, similar to [Crunch #1](crunch-1.md) or two guide RNAs (leading to heterotypic, homotypic, or monogenic perturbations).

Importantly, the number of guides received by a cell has an effect on its underlying transcriptomic distribution:

* two non-targeting guides (NC+NC) may have a different effect from a single non-targeting guide (NC);
* similarly, Gene A+NC may have a different effect from Gene A alone.

Although the dataset includes some cells that received three guides, the evaluation in this Crunch focuses exclusively on cells that received a single guide or two guides. For each cell, we provide its single-cell gene expression (RNA-seq) profile measured at day 14 of adipocyte differentiation, annotated with the identity of the perturbed genes, quality control (QC) metrics, and cell metadata.

The training dataset contains a subset of these perturbations, while a distinct set of perturbations is held out for validation and test.

The layout is as follow:

* The dataset is provided in [AnnData format (.h5ad)](https://anndata.readthedocs.io/en/stable/) as `obesity_challenge_2.h5ad`.
* Normalized gene expression values are stored in `adata.X`. Raw counts were normalized to a target sum of 100,000 per cell, followed by a $$log_2(1+x)$$ transformation (standard single-cell RNA-seq normalization; see [lecture 2 of the crash course](crash-course.md#lecture-2)).
* Raw gene expression counts prior to normalization are stored in `adata.layers['counts']` for reproducibility and alternative preprocessing.
* We provide data for cells that received single-guide, double-guide or three-guide perturbation. The perturbation target information for each cell is provided in `adata.obs['gene']`. Control cells, which receive perturbations with minimal transcriptomic effect, are labeled as `"NC"` (single-guide) or `"NC+NC"` (double-guide).
  * For perturbed cells, single-guide perturbations are indicated simply by the target gene name (e.g., 'Gene A').
  * All double-guide perturbations are denoted using a '+' format, which includes heterotypic gene pairs ('Gene A+Gene B'), homotypic pairs ('Gene A+Gene A'), and monogenic double-guide perturbations ('Gene A+NC').
  * Similarly, three-guide perturbations extend this format by linking three targets separated by a '+' (e.g., 'Gene A+Gene B+Gene C').
* Cell state/program enrichment information is provided in `.obs`, with columns `pre_adipo`, `adipo`, `lipo`, and `other` indicating whether each cell was enriched for pre-adipocyte, adipocyte, or lipogenic programs. `other` was defined as cells that were not enriched for either pre-adipocyte or adipocyte programs. Program enrichment assignments were based on expert-curated canonical signature genes, and the list of signature genes is provided in `signature_genes.csv`.
  * We provide the cell state proportion for each of the perturbations in a separate file `program_proportion.csv`.
* During preprocessing, standard single-cell quality control (QC) was applied to remove low-quality cells and cell doublets based on sequencing library complexity, gene detection rate, and mitochondrial gene content.

<details>

<summary>Definitions of the columns in <code>adata.obs</code></summary>

* `orig.ident`: The original sample ID.
* `nCount_RNA:` The number of UMIs detected per cell.
* `nFeature_RNA`: The number of genes detected per cell.
* `nCount_guide`: The number of sgRNA UMIs detected per cell.
* `nFeature_guide`: The number of sgRNAs detected per cell.
* `percent.mt`: The fraction of UMIs per cell that map to mitochondrial transcripts.
* `SampleID`: The sample ID.
* `Day`: The day of sample collection.
* `num_features`: The number of guides per cell (for low MOI data, after qc, only the cells with 1 guide are kept).
* `feature_call`: The guide assignment of each cell.
* `num_umis`: The number of guide umis per cell.
* `gene`: The perturbation target gene (or perturbation identity).
* `positive_control`: Whether the perturbation is one of the positive controls.

</details>

## Expected Output

Participants must submit three outputs:

### File: `prediction.h5ad`

An [AnnData](https://anndata.readthedocs.io/en/stable/) file containing predicted gene expression profiles normalized and log-transformed post-perturbation for 62 gene perturbations indicated in [`predict_perturbations_2.txt`](#user-content-fn-1)[^1]. across 36,601 genes listed in [`predict_genes_2.txt`](#user-content-fn-2)[^2].

Predictions should be stored in `adata.X` matrix with the corresponding perturbation identity recorded in `adata.obs['gene']`.

The set of genes (columns) included in the prediction is defined explicitly by `genes_to_predict` provided at inference time and the columns of `adata.X` must follow this order.

Note that the `predict_genes_2.txt` list may change between validation and test phases, and your model must generate predictions for whichever set of genes is supplied. The maximum number of genes that could be included in `genes_to_predict` is 36,601 corresponding to the total number of genes in the dataset.

For each gene perturbation, we ask you to predict the gene expression profiles for 100 cells to quantify the distribution of each perturbation prediction. With `N = len(predict_genes)`, the final prediction file is therefore required to have dimensions: \[6,200 × N] (cells × genes\_to\_predict).

### File: `predict_program_proportion.csv`

A [CSV](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html) file reporting the predicted proportion of cells with enriched programs for each gene perturbation listed in `predict_perturbations.txt`.

The file should contain one row per perturbation with the following columns:

* `gene`: should contain the perturbation name,
* `pre_adipo`, `adipo`, `lipo`, and `other`: should specify the predicted proportion of cells in each corresponding state for that perturbation,
* `lipo_adipo`: should be the ratio of `lipo` to `adipo` (representing the proportion of adipocytes with enriched lipogenic programs).

This file should thus have 62 rows and [6 columns](#user-content-fn-3)[^3]. An example is available in the `data/` directory.

### File: `Method description.md`

We ask you to please write a small document outlining the approaches used to generate both the predictions and the estimated proportions of cells enriched for each program.

This should include sufficient details of the computational models employed and the procedures used to derive cell proportions.

The document should be organized into three sections, represented as titles in a Markdown file:

* **Method Description**: Explain how your method works. _(5-10 sentences)_
* **Rationale**: Describe the reasoning behind your model. _(5-10 sentences)_
* **Data and Resources Used**: Specify the datasets and any other resources utilized. _(5-10 sentences)_

**Notes:**

* A human will validate the content at the end of the competition.\
  Work deemed unsatisfactory may be disqualified.
* This file must be **provided during submission**.\
  If content needs to be changed, you **must re-submit with the new version**.
* The name must be `Method Description.md`; case does not matter.&#x20;
* Only non-empty and non-comment lines are considered.

Below is an example of how to format the file:

{% code expandable="true" %}
```markdown
# Method Description

<!-- Explain how your method works. (5-10 sentences) -->
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Aliquam eget augue quis metus viverra vehicula sit amet lacinia odio.

# Rationale

<!-- Describe the reasoning behind your gene panel design. (5-10 sentences) -->
Praesent dignissim ipsum vel leo eleifend, eget pulvinar mauris ornare.
Duis efficitur lectus posuere iaculis dictum.

# Data and Resources Used

<!-- Specify the datasets and any other resources utilized. (5-10 sentences) -->
Donec feugiat eros vel odio gravida venenatis.
Nam et sem sit amet nisi vestibulum semper bibendum et libero.
```
{% endcode %}

{% hint style="info" %}
If there's an obvious issue regarding the format, you'll receive an immediate notification.

Notebook users are required to use [embedded files](../../participate/notebook-processor.md#embed-files).
{% endhint %}

## Scoring

Each metric will be displayed in a different leaderboard. Each will have a different ranking and opportunity for a prize.

The metrics are classed into 2 categories:

* Transcriptome-wide metrics that will be computed computed using **a subset of genes** (i.e., the columns of the predicted matrix) for each perturbation.
  * Metrics include:
    * **Pearson Delta** between predicted and observed perturbation effects relative to perturbed mean.
    * **Maximum mean discrepancy (MMD)** between predicted and observed distributions of single-cell profiles.
  * **Public leaderboard / validation** (updated weekly): Evaluation uses **1,000 hidden genes**.
  * **Private leaderboard / test phase**: Both the **number** and **identity** of scoring genes will remain unknown.
* A Program-level metric that will evaluate whether models capture meaningful biological outcomes, which is:
  * **L1-distance** between predicted and observed four cell state proportions for each perturbation (i.e. pre-adipogenic, adipogenic, lipogenic, and other)

{% hint style="info" %}
The evaluation code is available [on GitHub](https://github.com/crunchdao/competitions/blob/master/competitions/broad-obesity-2/scoring/scoring.py).

Code for local scoring will be available in the [quickstarter](https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/broad-obesity-2/quickstarters/perturbed-mean-baseline/perturbed-mean-baseline.ipynb).

For more details about how the metrics formulas, please consult the [Full Specifications](full-specifications.md).
{% endhint %}

## Submit

To build a valid submission, your model needs to be coded within the infer function, effectively respecting the crunch code submission interface.

{% code title="Python Notebook Cell" expandable="true" %}
```python
def train(
    data_directory_path: str,
    model_directory_path: str,
) -> None:
    """
    Train a perturbation prediction model.

    Parameters:
      data_directory_path: Directory where the data is located.
      model_directory_path: Directory where your model state should be persisted (usually named `resources/`).
    
    Return:
      None: Returned value is ignored.
    """
```
{% endcode %}

{% hint style="warning" %}
We recommend training locally and submitting weights because the dataset is large and cloud resources are limited.

Make sure that the [`Method description.md`](crunch-2.md#file-method-description.md) file properly documents your model, so that the Eric and Wendy Schmidt Center team can reference your work in their publications.
{% endhint %}

{% code title="Python Notebook Cell" expandable="true" %}
```python
def infer(
    data_directory_path: str,
    prediction_directory_path: str,
    prediction_h5ad_file_path: str,
    program_proportion_csv_file_path: str,
    model_directory_path: str,
    predict_perturbations: list[str],
    predict_genes: list[str],
):
    """
    Run inference for a set of perturbations.

    Parameters:
      data_directory_path: Path to the training AnnData file.
      prediction_directory_path: Directory where prediction files can be written.
      prediction_h5ad_file_path: Direct path where to write the `prediction.h5ad` file.
      program_proportion_csv_file_path: Direct path where to write the `predict_program_proportion.csv` file.
      model_directory_path: Directory containing your persisted model files.
      predict_perturbations: The perturbations for which to generate predictions.
      predict_genes: List of gene names (columns) to include in the prediction.h5ad AnnData object.

    Return:
      None: Returned value is ignored.

    Expected files:
      prediction_directory_path / "prediction.h5ad": anndata.AnnData
            AnnData matrix of size (n_perturbations * cells_per_perturbation, n_genes_to_predict), containing the predicted gene expression values.
      prediction_directory_path / "predict_program_proportion.csv": pd.DataFrame
            DataFrame (index=False) containing estimated cell-type proportions for each perturbation.
    """
```
{% endcode %}

{% hint style="info" %}
An example is available in the [quickstarter](https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/broad-obesity-2/quickstarters/perturbed-mean-baseline/perturbed-mean-baseline.ipynb).
{% endhint %}

## FAQ

<details>

<summary>I missed Crunch 1, can I participate to this one?</summary>

Yes.

All crunches are unique. There is no requirement to have participated in the first one in order to participate in the others.

</details>

<details>

<summary>I participated to Crunch 1, can my model be adapted?</summary>

Yes.

The model can be adapted with minimal modifications because:

* Some file names have changed:
  * The prefix `_2` has been added.
  * They are now matching the specifications more closely:\
    `perturbations_to_score.txt` is now `score_perturbations_2.txt`&#x20;
* Some parameters have been renamed to reflect the file names.\
  The old names are still available for convenience.
* There are more masks (see the quickstarter):
  * `single_perturb_mask`
  * `double_perturb_mask`
  * `overall_perturb_mask`

</details>

<details>

<summary>I missed a checkpoint, can I participate to the next one?</summary>

Yes.

There are checkpoints every Monday, and missing one will not affect your final ranking. Once your model is ready, submit it!

</details>

<details>

<summary>Why must external resources be published or in the public domain?</summary>

While releasing the full model is encouraged, it is not strictly required if the weights are sufficient for reproducibility.

When constraints limit what can be released, we ask that the methods and training procedures be clearly documented. These cases can be reviewed individually to ensure transparency and fairness.

</details>

[^1]: File is available in the `data/`  directory.

[^2]: File is available in the `data/` directory.

[^3]: Use `.to_csv(index=False)` when saving the file.
