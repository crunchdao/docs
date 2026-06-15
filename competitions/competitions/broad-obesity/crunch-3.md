---
description: >-
  Nominate combinatorial gene perturbations that are predicted to most strongly
  increase a target transcriptional program.
---

# Crunch 3 – Identifying combinatorial perturbations to drive adipocyte differentiation

## TL;DR;

* **The Goal**: Find pairs of genes that, when switched off together, make fat cells burn energy instead of storing it.
* **The Task**: Score and rank \~4.47 million candidate pairs by how strongly they trigger fat-burning. Top picks get tested in a real lab experiment after the competition.
* **The Scoring**: 12 fat-burning scores plus one overall score per pair.

### What's new vs Crunch 1 and 2

* **Scale**: Crunch 1 tested single genes. Crunch 2 tested \~170 pairs from an 18-gene shortlist. Crunch 3 covers 4.47M pairs across 2,991 genes.
* **Output**: Crunch 1 and 2 wanted detailed cell-by-cell predictions. Crunch 3 wants one ranking score per pair.
* **Feedback**: Crunch 1 and 2 had a weekly public leaderboard. Crunch 3 **has no scoring** during the competition. The final leaderboard arrives after the lab experiment.

## Evaluation Phases

In Crunch 3, you will not have the opportunity to evaluate your model's predictive performance. A final leaderboard will only be published after a lab experiment.

**All participants may submit up to two times:**

* For the first submission window, **participants from Crunch 2 are especially encouraged** to **adapt their existing models and submit** them for Crunch 3 within the first 3 weeks of the competition.&#x20;
* After that, **all participants, including both Crunch 2 participants and new participants**, will have an **additional 3 weeks to submit or revise** their work before the competition closes.

This division will enable the EWSC team to conduct a preliminary selection based on the results of Crunch 2.

### Timeline

There will be multiple quota refresh, with one occurring every Tuesday at 4:00 p.m. UTC:

* Refresh 1 – May 19th
* Refresh 2 – May 26th
* Refresh 3 – June 2nd
* **Proposal 1 deadline – June 9th**
  * If no selection is made, the last proposal is used.
  * There will be a one-day downtime.
* Refresh 5 – June 16th
* Refresh 6 – June 23th
* Refresh 7 – June 30th
* **Proposal 2 deadline – July 3rd**
  * If no selection is made, the last proposal is used.
  * If the same as Proposal 1, it is ignored.
* Peer review begins – July 4th
  * More information will be provided later.
* **Peer review ends – July 10th**

## Overview

Participants must predict the effect of two-gene perturbations on thermogenesis-related gene expression in adipocyte cells.

The search space covers all possible pairs drawn from 2,991 genes (those with >5 transcripts per million, plus transcription factors and marker genes), yielding \~4.47 million gene pairs (excluding pairs already tested in Crunch 2).

Participants can build on work from Crunches 1 and 2, but it's not required. **Newcomers are also welcome** and may use any approach they choose, including modeling, biological knowledge, public resources, or other strategies.

### Objective

The objective in Crunch 3 is to predict the mean z-scored enrichment score for each of the 12 thermogenic gene sets per perturbation, and then to derive the final score by averaging the top 3 scores per perturbation to rank perturbations from highest to lowest.

Therefore, for each perturbation, participants should predict 13 scores (12 gene-set-level scores and 1 final aggregated score).

<figure><img src="../../../.gitbook/assets/image (152).png" alt=""><figcaption><p>Overall Workflow</p></figcaption></figure>

## Dataset

For the training datasets released in Crunches 1 (`TF150`) and 2 (`TF15_MOI2`), we will provide the raw and z-scored signature scores for each adipocyte cell in [`{...}_ThermoScores_cell.csv` files](#user-content-fn-1)[^1]:

* Each row is a cell, identified by `cell_id` and its assigned perturbation (under column `gene`).&#x20;
* The raw enrichment score for each of the 12 signatures appears as a [named columns](#user-content-fn-2)[^2], and the corresponding z-score computed across all adipocyte cells is provided in the [`z_`-prefixed columns](#user-content-fn-3)[^3].

The perturbation-level score for each perturbation is provided in [`{...}_ThermoScores_perturbation.csv` files](#user-content-fn-4)[^4]:

* Each row is a perturbation.&#x20;
* The signature columns (named according to the signature gene sets, 12 in total) contain the mean z-score across all cells belonging to that perturbation.&#x20;
* The column `agg_top3_z` is the final aggregated score (the mean of the top 3 signature z-scores) used to rank perturbations.

The datasets come from two separate experiments and were processed separately. Therefore, the absolute expression values may differ across the two datasets, even though they are z-normalized. When combining them, participants are free to decide how to combine or adjust them if needed.

The datasets includes cells with more than two perturbations, but the new experiment mainly focuses on double perturbations. When computing the threshold to beat using the existing data, we require more than 16 adipocyte cells for a perturbation to receive a valid score.

The signature gene sets used to define the score can be found in `thermogenic_signatures.csv`. The notebook showing how the scoring procedure was derived and computed is [available here](https://julielaffy.github.io/obesity-broad-ml-competition-2025/program_analysis_py.html#Part-4:-Thermogenic-Scoring).

### Crunch 1 & 2 Training Data

The Crunch 1 and Crunch 2 training datasets are available via the `TF150.h5ad` and `TF15_MOI2.h5ad` files, respectively. These are the same files provided in previous parts, just renamed for consistency with the thermo score files.

Since the files are large, a smaller version of the data is available to make it easier and faster to get and load the data. In the cloud environment, only the "default" one will be available.

You can find out more about the content of the training data for [Crunch 1 here](https://docs.crunchdao.com/competitions/competitions/broad-obesity/crunch-1#dataset) and for [Crunch 2 here](https://docs.crunchdao.com/competitions/competitions/broad-obesity/crunch-2#dataset).

## Expected Output

Your submission should include the following:

### File: `prediction.parquet`

A [PARQUET](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_parquet.html) file containing all candidate gene pairs, with one row per gene pair and at least the following columns:

* `GenePairID`: **Gene pair ID**, in the format `GeneA+GeneB` or `GeneA+GeneA`.\
  The IDs should match the pairs in `predict_perturbations_3.parquet` file.
* `nonucp1.all`,
* `REACTOME_AMPK_inhibits_chREBP_R-HSA-163680`,
* `REACTOME_Integration_of_energy_metabolism_R-HSA-163685`,
* `REACTOME_Mitochondrial_biogenesis_R-HSA-1592230`,
* `emont.hAd6`,
* `lit.thermogenic`,
* `C2.WP_THERMOGENESIS`,
* `C5.GOBP_ADAPTIVE_THERMOGENESIS`,
* `C5.GOBP_BROWN_FAT_CELL_DIFFERENTIATION`,
* `C5.GOBP_NEGATIVE_REGULATION_OF_COLD_INDUCED_THERMOGENESIS`,
* `C5.GOBP_POSITIVE_REGULATION_OF_COLD_INDUCED_THERMOGENESIS`,
* and `yi.Thermogenesis_Village`: **Predicted scores for each of the 12 thermogenic signatures**, with one column per signature.\
  Column names should match the signature names in the `thermogenic_signatures.csv` file.
* `FinalAggScore`: **Predicted final aggregated score for the gene pair**, defined as the mean of the top-3 predicted signature scores among the 12 signatures.
* `Rank`: **Predicted rank** of the gene pair based on the final aggregated score.

This file should thus have 4,474,413 rows and [15 columns](#user-content-fn-5)[^5].

### File: `Report.md`

We ask you to please write a small document that should include sufficient details outlining the approaches used by your algorithm to compute the scores.

The document should be organized into three sections, represented as titles in a Markdown file:

* **Method description**: Describe the algorithm used to generate the predicted scores. _(5-10 sentences)_
* **Design rationale**: Explain the rationale behind the proposed gene panel design. _(5-10 sentences)_
* **Data and Resources**: Describe the datasets, prior knowledge, and any additional resources used. _(5-10 sentences)_
* A reference list may be included.

**Notes:**

* A human will validate the content at the end of the competition.\
  Work deemed unsatisfactory may be disqualified.
* This file must be **provided during submission**.\
  If content needs to be changed, you **must re-submit with the new version**.
* The name must be `Report.md`; case does not matter.&#x20;
* Only non-empty and non-comment lines are considered.

Below is an example of how to format the file:

```markdown
# Method Description

<!-- Describe the algorithm used to generate the predicted scores. (5-10 sentences) -->
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Aliquam eget augue quis metus viverra vehicula sit amet lacinia odio.

# Design rationale

<!-- Explain the rationale behind the proposed gene panel design. (5-10 sentences) -->
Praesent dignissim ipsum vel leo eleifend, eget pulvinar mauris ornare.
Duis efficitur lectus posuere iaculis dictum.

# Data and Resources

<!-- Describe the datasets, prior knowledge, and any additional resources used. (5-10 sentences) -->
Donec feugiat eros vel odio gravida venenatis.
Nam et sem sit amet nisi vestibulum semper bibendum et libero.
```

## Requirements

Your algorithm should include an `infer()` function that returns predictions on the test set.

The prediction file should not exceed 1 GB. Use `int32` and `float32` to optimize the size.

The execution time of your algorithm should not exceed the platform's time limits: **10 hours per week**.

## Evaluations

For this part, there will be no direct feedback that participants can use to judge their ranking against others', as scores will only be known after experimental validation is complete.

The leaderboard will only show users (or teams) who submitted an algorithm that can generate a valid prediction.

The scoring, and thus the prize, is divided into two parts:

### Evaluation 1 – Peer-review

Evaluation 1 will award **10% of the prize pool** and is focus on the submitted algorithms and rationales used to select gene pairs for experimental testing.

These submissions will be assessed through a [peer review](crunch-3.md#peer-review) process, and winners for this evaluation will be announced at the **end of Summer 2026**.

{% hint style="info" %}
A proposal will only be considered if its author has also peer-reviewed other proposals.
{% endhint %}

### Evaluation 2 – Wet lab experiment

Evaluation 2 will award **90% of the prize pool** and will assess algorithmic performance after new laboratory experiments have been completed.

Specifically, all submitted algorithms will be evaluated based on how accurately they predict the effects of combinatorial perturbations using the newly generated experimental data. Winners based on this evaluation will be announced in **Fall 2026 after the new laboratory experiments have been performed**.

{% hint style="info" %}
Prizes will only be awarded to the top 10 teams whose proposed gene pairs achieve experimentally validated scores that exceed the maximum perturbation score observed in the training data released from Crunches 1 and 2.
{% endhint %}

## Peer Review

The peer review component is a mandatory part of the competition. In order to qualify for prizes, you must review 3-4 submissions from other participants.

The peer review process is crutial for selecting the most promising gene panels for [experimental validation](crunch-3.md#experimental-validation). Your evaluations help identify submissions with strong justifications and innovative approaches.

For each assigned submission, reviewers should:

* Assign an overall rating on a 1-3 scale:
  * `1` – poor proposal / justification
  * `2` – adequate proposal / justification
  * `3` – excellent proposal and justification
* Provide a short written evaluation of 200-400 words covering:
  * Rationale of the design.
  * Novelty of the design.
  * Compliance with the required format.

## Experimental Validation

To validate the most promising gene panels, **we will select a total of 150 unique gene pairs for experimental evaluation**. This selection will occur via two routes:

### Route 1: nominations from Crunch 2 winners

A total of \~110 unique gene pairs will be selected from top-performing teams in **Crunch 2 that also participate in Crunch 3**.

Selection will proceed as follows:

* The 30 top-performing teams from Crunch 2 (top 10 teams for each of 3 evaluation metrics) will be considered.
* For each team, the top 4-6 gene pairs from that team’s first Crunch #3 submission will be used.

This process yields up to 180 nominations, which will be consolidated to \~110 unique gene pairs after accounting for overlap across evaluation metrics and teams.

### Route 2: nominations from Crunch 3 winners

A total of \~40 unique gene pairs will be selected from top-performing Crunch 3 submissions based on the [peer review](crunch-3.md#peer-review).

Selection will proceed as follows:

* The top 10 teams will be identified based on peer-review rankings.
* For each selected team, the top 4-6 gene pairs will be taken from the winning proposals.

This process yields up to 60 nominations, which will be consolidated to \~40 unique gene pairs after accounting for overlap.

Together, these two routes define the full experimental validation set of 150 gene-pair combinations.

## FAQ

<details>

<summary>I missed the other crunches, can I participate to this one?</summary>

Yes.

All crunches are unique. You don't need to have participated in the first or second one to participate in the last one.

</details>

<details>

<summary>I participated to Crunch 2, can my model be adapted?</summary>

Yes.

In Crunch 2, you had to predict the perturbation of gene pairs. In Crunch 3, you must do the same and then rank them.

Updating your one-page report is also very crutial so the EWSC team can understand your reasoning.

</details>

<details>

<summary>Why must external resources be published or in the public domain?</summary>

While releasing the full model is encouraged, it is not strictly required if the weights are sufficient for reproducibility.

When constraints limit what can be released, we ask that the methods and training procedures be clearly documented. These cases can be reviewed individually to ensure transparency and fairness.

</details>

[^1]: `TF150_ThermoScores_cell.csv` and `TF15_MOI2_ThermoScores_cell.csv`

[^2]: e.g. `C2.WP_THERMOGENESIS`

[^3]: e.g. `z_C2.WP_THERMOGENESIS`

[^4]: `TF150_ThermoScores_perturbation.csv` and `TF15_MOI2_ThermoScores_perturbation.csv`

[^5]: Use `.to_parquet(index=False)` when saving the file.
