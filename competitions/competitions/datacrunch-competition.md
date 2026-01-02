---
description: >-
  This weekly prediction contrast ranks 3000 most liquid US equities for
  DataCrunch's Hedge Fund.
cover: ../../.gitbook/assets/banner (1).webp
coverY: 0
---

# DataCrunch Competition

## Overview

DataCrunch uses the quantitative research of the CrunchDAO to manage its systematic market-neutral portfolio. DataCrunch built a dataset covering thousands of publicly traded U.S companies.

The long-term strategic goal of the fund is capital appreciation by capturing idiosyncratic return at low volatility.

In order to achieve this goal, DataCrunch needs the community to assess the relative performance of all assets in a subset of the [Russell 3000](https://www.investopedia.com/terms/r/russell_3000.asp) universe. In other words, DataCrunch is expecting your model to rank the constituent of its investment universe.

## Prize

Reward are split in targets as follow. Each target represent an investment horizon and can be predicted using the DataCrunch dataset. Reward are distributed every month based on crunchers performance:

* 60,000 $USDC yearly on target\_b + $10k bonus for cumulative alpha target.
* 20,000 $USDC yearly on target\_g
* 20,000 $USDC yearly on target\_r
* 10,000 $USDC yearly on target\_w

## Weekly Crunches

Every week, two phases:

* The `Submission Phase:` every Friday at 8PM UTC to Tuesday 12PM UTC, the system will release an additional `moon`. Competitors will be able to submit their code or model
* The `Out-Of-Sample Phase:` the models will be run on the Out-Of-Sample data (the live data). The score of each target are published as they are resolved against live market data as reward.

## Data

Each row of the dataset describes a stock at a certain date.

The dataset is composed of three files, `X_train y_train and X_test`.

### X\_train

* `moon`: A sequentially increasing integer representing a date. Time between subsequent dates is constant, denoting a weekly fixed frequency at which the data is sampled.
* `id`: A unique identifier representing a stock at a given Moon. Note that the same asset has a different `id` in different Moons.
* `Feature_Industry`: the industry to which a `stock` belongs at a given `moon.`
* (`gordon_Feature_1`, …, `dolly_Feature_30`): Anonymised `features` that describe the state of assets on a given date. They are grouped into several families, or ways of assessing the relative performance of each stock on a given month.

Note: All features have the string "Feature" in their name, prefixed by a code name for the feature family.

### y\_train

* `moon`: Same as in `X_train`.
* `id`: Same as in `X_train`.
* (`target_w`, …, `target_b`): the targets that may help you build your models. Target\_w, r, g, b refer to 7, 28, 63, 91 days compounding of returns.&#x20;

### X\_test - y\_test

`X_test` and `y_test` has the same structure as `X_train` and `y_train` but comprises only 13 moons. These files are used to simulate the submission process locally via `crunch.test()` (within the code), or `crunch test` (via the cli). The aim is to help participants debug their code and have successful submissions. A successful local test usually means no errors during execution on the submission platform. The data of these files is composed of the 13 moons on which the longest target (target\_b) is not resolved. The missing data for each target were replaced by -1 values.

_Note_: the features are split in two groups. The legacy features and the v2 features which are suffixed by "\_v2"

## The Performance Metric

The infer function from your code will return your predictions.&#x20;

A Spearman rank correlation will be computed against the live targets.&#x20;

### Reward Scheme

All reward are computed on the leaderboards.

<figure><img src="../../.gitbook/assets/Reward Scheme.png" alt=""><figcaption></figcaption></figure>

The **Hist**orical **Rewards** are the sum of every payout you have received from the DataCrunch competition.

The **Proj**ected **Rewards** are the current estimated rewards yet to be distributed.

## Payouts calculation

Payouts are computed based on your the rank of your prediction for each target. The higher the Spearman Rank between your prediction and market realisation, the higher your rank on the leaderboard.&#x20;

The payouts are distributed according to an exponential function of your position on the leaderboards, as shown in the graph below, the top 20 crunchers earn approximately 30% of the total rewards.

<figure><img src="../../.gitbook/assets/Payouts Calculation.png" alt=""><figcaption><p>Cumulative distribution of rewards (2023-03-03 leaderboard)</p></figcaption></figure>

## Computing Resources

Competitors will be allocated a specified quantity of computing resources within the cloud environment for the execution of their code.&#x20;

During the phase, they are entitled to **10 hours** of GPU or CPU compute time per week, and for the `OOS` phase, this allocation increased 10% in case of slower deployement by the system.

During the `SUBMISSION`  phase, you are entitled to 10 hours of GPU or CPU computing time per week, and during the OOS phase, this allocation is increased by 10%.

## Quickstarter Notebook

A Quickstarter notebook is available below so you can get familiar with what is expected from you.

{% embed url="https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/datacrunch/quickstarters/quickstarter/quickstarter.ipynb" %}

## Legacy Endpoints

{% hint style="warning" %}
The old data format is still available on the legacy endpoint, but will be removed at some point in the future. We encourage people who still rely on this data to migrate to the new submission format.
{% endhint %}

<table><thead><tr><th width="233">Name</th><th width="304">Parquet</th><th width="266">CSV (deprecated)</th></tr></thead><tbody><tr><td><code>X_train</code></td><td><a href="https://tournament.crunchdao.com/data/X_train.parquet">/data/X_train.parquet</a></td><td><a href="https://tournament.crunchdao.com/data/X_train.csv"><del>/data/X_train.csv</del></a></td></tr><tr><td><code>y_train</code></td><td><a href="https://tournament.crunchdao.com/data/y_train.parquet">/data/y_train.parquet</a></td><td><a href="https://tournament.crunchdao.com/data/y_train.csv"><del>/data/y_train.csv</del></a></td></tr><tr><td><code>X_test</code></td><td><a href="https://tournament.crunchdao.com/data/X_test.parquet">/data/X_test.parquet</a></td><td><a href="https://tournament.crunchdao.com/data/X_test.csv"><del>/data/X_test.csv</del></a></td></tr><tr><td><code>example_submission</code></td><td><a href="https://tournament.crunchdao.com/data/example_submission.parquet">/data/example_submission.parquet</a></td><td><a href="https://tournament.crunchdao.com/data/example_submission.csv"><del>/data/example_submission.csv</del></a></td></tr></tbody></table>
