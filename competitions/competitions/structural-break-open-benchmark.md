---
description: New edition of the ADIA Lab Structural Break Challenge with a new dataset.
cover: >-
  https://raw.githubusercontent.com/crunchdao/competitions/refs/heads/master/competitions/structural-break/assets/banner.webp
coverY: 0
---

# ADIA Lab Structural Break Open Benchmark Challenge

## Overview

The ADIA Lab Structural Break Open Benchmark Challenge is a continuation of the [original ADIA Lab Structural Break Challenge](adia-lab-structural-break-challenge.md).

The benchmark addresses the same scientific problem, uses data with the same structure and characteristics, and preserves the original evaluation philosophy. It extends the original challenge into a long lived, continuously evaluated public benchmark.

## Scientific Continuity

The open benchmark preserves:

* The original research question
* The data generating philosophy
* The evaluation metric
* The definition of structural breaks

The objective is not to reset the problem, but to extend it over time under comparable conditions.

## Changes Introduced

Originality Constraint:

* An originality metric has been introduced to prevent solution cloning and excessive convergence.
* Submissions that are highly correlated with any of the original top models will be rejected.

Leaderboard Initialization:

* The top ten models from the original competition are visible on the leaderboard as fixed reference benchmarks.

Out of Sample Evaluation Schedule:

* Models are evaluated on a rolling out of sample basis.
* Scoring occurs at the end of each calendar quarter, starting with Q1 2026.  \
  The first official scoring date is March 31st, 2026.

## Possible Use

This benchmark is intended for:

* Academic research on structural breaks
* Method comparison under regime change
* Long horizon robustness evaluation

It is designed to remain open and relevant beyond a single competition cycle.

## Problem Description

Participants are asked to build models that determine whether a **structural break has occurred at a known breakpoint** in a univariate time series.

A structural break corresponds to a change in the underlying data generating process. Such breaks are common in economic and financial time series and can significantly impact inference, forecasting, and decision making.

Each time series consists of two segments separated by a known boundary. The task is to assess whether the statistical properties of the two segments differ in a way consistent with a structural break.

<figure><img src="../../.gitbook/assets/image1.png" alt=""><figcaption><p>Structural Break Example</p></figcaption></figure>

## Timeline

* **Opening of the new competition**: December 18, 2025
* **Quarterly Out-of-Sample**: March 31, 2026
* **Quarterly Out-of-Sample**: June 30, 2026
* **Quarterly Out-of-Sample**: September 30, 2026
* **Quarterly Out-of-Sample**: December 31, 2026

{% hint style="info" %}
A downtime of one week will be necessary to compute the Out-of-Sample score.
{% endhint %}

## Evaluation

For each time series in the test set, your task is to predict a score between `0` and `1`, where values towards `0` mean that no structural break occurred at the specified boundary point, and values towards `1` mean that a structural break did occur. The evaluation metric will be the [ROC AUC (Area Under the Receiver Operating Characteristic Curve)](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html), which measures the performance of detection algorithms regardless of their specific calibration.

A ROC AUC value around `0.5` means that the algorithm is not able to detect structural breaks better than random chance, while values approaching `1.0` indicate perfect detection. The ROC AUC allows us to compare the output of different detection methods by removing the specific bias of each method towards false positives or false negatives.

The competition follows a two-stage evaluation process:

1. **Public Leaderboard**: Each submission is immediately scored against a portion of the test data, and results appear on the public leaderboard;
2. **Private Leaderboard**: At the end of the competition, selected submissions are evaluated on the remaining test data to determine the final rankings.

This approach ensures that models are evaluated on their ability to generalize rather than potentially overfitting to the public leaderboard data.

## Code Submission

This is a code competition where participants are required to submit their Python code (files or notebooks) directly to the CrunchDAO platform. Your submission should:

1. Process and analyze the data;
2. Output a score between `0` and `1` for each time series `id` in the test set, representing the likelihood of a structural break;
3. Your code must produce deterministic output, or it will be ineligible for any rewards;
4. Only the team leader will be ranked on the leaderboard, and be eligible for a reward.

Your submitted code will be executed on the competition platform and automatically scored against a portion of the test set. Shortly after submission, your score will appear on the public leaderboard of the competition.

## Submission Requirements

* Your main solution file should follow the template provided by the competition host here[^1];
* Your solution must include `train()` and `infer()` functions. The first one is meant to train your model on the training set, in case your model needs that, otherwise it can be left empty. The second one takes the test data as input and returns predictions;
* The execution time of your solution should not exceed the platform's time limits: 15 hours per week.

{% hint style="success" %}
The `yield`-based approach is now optional.\
If no such use is detected, the entire `X_test` will be provided as a `DataFrame`.
{% endhint %}

## Dataset Description

The dataset for this competition comprises tens of thousands of synthetic univariate time series, each containing approximately 1,000 to 5,000 values with a designated boundary point. For each training time series, a label (`True` for break, `False` for no break) indicates whether a structural break occurred at this boundary point.

The time series in this competition are designed to represent various real-world scenarios where structural breaks may occur, with different levels of difficulty in detection. This includes scenarios similar to those found in financial markets, climate data, industrial sensor readings, and biomedical signals, among others. The challenge is to develop algorithms that can generalize across these scenarios and accurately detect structural breaks in new, unseen data.

### Data Format

For the training data, the `X` variable will be provided as a `pandas.DataFrame` with a `MultiIndex` structure. Here's an example of the format:

```
                  value  period
id     time
0      0       0.001858      0
       1      -0.001664      0
       2      -0.004386      0
       3       0.000699      0
       4      -0.002433      0
...              ...        ...
10000  1890   -0.005903      1
       1891    0.007295      1
       1892    0.003527      1
       1893    0.007218      1
       1894    0.000034      1
```

The `DataFrame` has the following structure:

* A `MultiIndex` with two levels:
  * `id`: Identifies the unique time series (each time series has a unique ID)
  * `time`: The timestep within each time series
* The columns include:
  * `value`: The actual time series value at that timestep;
  * `period`: A binary indicator where `0` represents the period before the boundary point, and `1` represents the period after the boundary point. The structural break occurs at the change in value, but it may take some time (values) to become detectable/apparent.

The `y` variable is a boolean `pandas.Series`, with `id` as index, indicating whether a structural break\
occurred at the boundary point for that time series (`True` if there was a break, `False` otherwise).

The test data will follow the same format. Your code will need to process these time series and generate predictions of the likelihood of a structural break for each unique time series ID.

### Data Size

The training set consists of 10,000 datasets and is the same as last year's.

There will also be another 10,000 new datasets for each of the [public](../../other/glossary.md#submission-phase) and [private](../../other/glossary.md#out-of-sample-phase) test sets.

The testing data provided for local usage only consists of 100 datasets. Participants must consider that their code will run on **100 times more** datasets, with a maximum limit of 15 hours of computing time.

A determinism check will re-run the infer function on 30% of the data (3,000 datasets) to ensure your model is deterministic. The results must be equal, with a tolerance of `1e-8`.

### Comparison against the Top 10

The participants with the highest scores in the [original challenge](https://hub.crunchdao.com/competitions/structural-break/leaderboard) will be used as fixed reference points. These models will only be used as a baseline for benchmarking purposes and will not be eligible for prizes.

| Leaderboard Rank | Cruncher(s)                                                                                                                                                                                                                                                                                                            | ROC AUC Score |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| 1st place        | <p><a href="https://hub.crunchdao.com/users/brandao">Humberto Brandão</a><br><a href="https://hub.crunchdao.com/users/mario-filho">Mário Augusto Filho</a><br><a href="https://hub.crunchdao.com/users/rafael">Rafael Alencar</a><br><a href="https://hub.crunchdao.com/users/joao-peinado">João Pedro Peinado</a></p> | 90.14%        |
| 2nd place        | [Farukcan Saglam](https://hub.crunchdao.com/users/farukcan-saglam)                                                                                                                                                                                                                                                     | 89.85%        |
| 3rd place        | [Lucas Morin](https://hub.crunchdao.com/users/lcrm)                                                                                                                                                                                                                                                                    | 89.59%        |
| 4th place        | [Julian Mukaj](https://hub.crunchdao.com/users/valuable-j)                                                                                                                                                                                                                                                             | 89.32%        |
| 5th place        | <p><a href="https://hub.crunchdao.com/users/guoqin-gu">Guoqin Gu</a><br><a href="https://hub.crunchdao.com/users/mutian-hong">Mutian Hong</a></p>                                                                                                                                                                      | 89.28%        |
| 6th place        | [Leo Huu Dung Nguyen](https://hub.crunchdao.com/users/leo-nguyen)                                                                                                                                                                                                                                                      | 89.17%        |
| 7th place        | [Tuah Jihan](https://hub.crunchdao.com/users/tuah-jihan)                                                                                                                                                                                                                                                               | 88.34%        |
| 8th place        | [Abhishek Gupta](https://hub.crunchdao.com/users/abhishek-gupta)                                                                                                                                                                                                                                                       | 87.78%        |
| 9th place        | [Arina Streltsova](https://hub.crunchdao.com/users/alright-jho)                                                                                                                                                                                                                                                        | 87.72%        |
| 10th place       | [Rakesh Jarupula](https://hub.crunchdao.com/users/stealth-rakesh)                                                                                                                                                                                                                                                      | 86.72%        |

{% hint style="info" %}
A Spearman correlation test will be used to assess similarity with the Top 10 models. Models deemed too similar, with an out of sample correlation above 95 percent, will not be eligible for rewards.
{% endhint %}

{% hint style="info" %}
[You can watch the Talks and Award Ceremony here.](https://www.youtube.com/watch?v=LFWdWgAcOqU)
{% endhint %}

## Methodology Suggestions

Methods such as change point detection algorithms, tests for equality of distributions, anomaly detection, or supervised learning models can be utilized to recognize patterns associated with structural breaks. Some approaches to consider include:

* Statistical tests comparing the distributions before and after the boundary point;
* Feature extraction from both parts of the time series for comparative analysis;
* Time series modeling to detect deviations from expected patterns;
* Deep learning approaches for automated pattern recognition.

Careful preprocessing of the time series data is an essential step in developing robust detection models.

The ultimate goal is to develop reliable algorithms for detecting structural breaks in time series data across various domains where such changes have significant implications for decision-making and risk management.

## Prizes

| Winners’ rank | Prize value per quarter |
| ------------- | ----------------------- |
| 1st place     | $3,000 USD              |
| 2nd place     | $2,000 USD              |
| 3rd place     | $1,000 USD              |

Scoring occurs at the end of each calendar quarter, starting with Q1 2026.

## FAQ

<details>

<summary>What is a Structural Break?</summary>

A structural break in a time series can be defined explicitly as an alteration in the underlying [data-generating process (DGP)](structural-break-open-benchmark.md#intervention-on-the-data-generating-process-dgp), or implicitly through illustrative examples of series exhibiting structural breaks versus those that do not.

#### Intervention on the Data-Generating Process (DGP)

Formally, a structural break occurs at a specific time point when the characteristics of the DGP governing a time series change. For instance, consider a random walk characterized by parameters such as drift `μ` and volatility `σ`. A structural break is said to occur at the moment one or more of these parameters experience a change - for example, volatility `σ` changing from `1.0` to `2.0` at a certain point in time.

Structural breaks need not always involve explicit mathematical equations or parameter adjustments. Another scenario might involve constructing a single time series by combining segments with inherently different behaviors or data sources - for instance, measuring daily temperature changes with one thermometer initially, then switching to a different thermometer after a specified breakpoint.

In short, a structural break is explicitly characterized by a change in the fundamental nature or parameters of the DGP. This change can be either abrupt or smooth and could manifest as parameter changes, functional form changes, regime transitions, or combinations thereof. If no such change occurs, the series does not contain a structural break.

#### Implicit Definition via Examples

An alternative approach involves implicitly defining a structural break through examples. Under this approach, a substantial and diverse collection of time series, some exhibiting structural breaks and others remaining stable, can implicitly define the concept.

Such example-based definitions are particularly useful in machine learning and statistical inference contexts, where explicit parameter-level descriptions might not be directly available or practical. This is especially relevant when working with real-world data, where the underlying data generation process is typically unknown. Importantly, failing to account for structural breaks in time series analysis can lead to misleading results, including inaccurate forecasts and invalid statistical inferences.

</details>

<details>

<summary>Can I reuse my old code from the original competition?</summary>

**Yes**, you can reuse your old code, but the **originality constraint** will be applied.

This means that if your model or approach closely resembles one of the original top 10 models from the original competition, it may not be accepted for new submissions. The goal is to foster innovation and prevent exact replications of previous solutions.

</details>

<details>

<summary>How is originality measured?</summary>

Originality is measured using a spearman correlation that compares your model's predictions with those of the original top models.

If your submission is highly correlated with any of the original models, it will be flagged and will not be accepted. This ensures that the solutions in the benchmark remain innovative and not overly similar to previous submissions.

</details>

<details>

<summary>How many datasets will my <code>infer()</code> function be called with?</summary>

A total of 16,000.

The details are as follows:

* 3,000 for the top 10 correlation check,
* 10,000 for the regular dataset,
* and 3,000 for the determinism check.

Everything must fit within the 15-hour quota.

</details>

[^1]: TODO Use correct link
