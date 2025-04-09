---
icon: lock
cover: ../../.gitbook/assets/banner (7).webp
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

# DataCrunch Rally

## Overview

DataCrunch uses the quantitative research of the CrunchDAO to manage its systematic market-neutral portfolio. DataCrunch built a dataset covering thousands of publicly traded U.S companies.

The long-term strategic goal of the fund is capital appreciation by capturing idiosyncratic return at low volatility.

In order to achieve this goal, DataCrunch needs the community to assess the relative performance of all assets in a subset of the [Russell 3000](https://www.investopedia.com/terms/r/russell_3000.asp) universe. In other words, DataCrunch is expecting your model to rank the constituent of its investment universe.

This rally is a new iteration on DataCrunch's dataset called master-v3.

## Prize

The total cash prize for the rally is 10,000 $USDC, equivalent to 3,125 $CRUNCH. Winners will be able to choose their prize format, $CRUNCH or $USDC.&#x20;

## Rally Phases And Format

The Rally is formatted in two phases:

* The `Submission Phase:` competitors will have 1 month to build, test and submit their model. Once they are satisfied with it, they will need to select it in order to compete in the `Out-Of-Sample` phase.
  * Dates: March, 15th - April, 29th 11:59:59PM CET 2024.
* The `Out-Of-Sample Phase:` the models will be run on the Out-Of-Sample data for four weeks, without any possibility to modify the submission. The scores will be released each week on Fridays.
  * Final Leaderboard Date: May, 24th 2024.

## Data

Each row of the dataset describes a stock at a certain date.

The dataset is compose of three files, `X_train y_train and X_test`.

### X\_train

* `Moons`: A sequentially increasing integer representing a date. Time between subsequent dates is constant, denoting a weekly fixed frequency at which the data is sampled.
* `id`: A unique identifier representing a stock at a given Moon. Note that the same asset has a different `id` in different Moons.
* `Feature_Industry`: the industry to which a `stock` belongs at a given `moon`. To ensure market neutrality, DataCrunch limits exposure to industry bets by cross-sectionally neutralizing model output before scoring. Therefore, it is important that your model does not bet on any particular industry but provides accurate performance estimates across industries.
* (`Gordon_Feature_1`, …, `Dolly_Feature_30`): Anonymised `features` that describe the state of assets on a given date. They are grouped into several families, or ways of assessing the relative performance of each stock on a given month.

Note: All features have the string "Feature" in their name, followed by a code name for the feature family.

### y\_train

* `Moons`: Same as in `X_train`.
* `id`: Same as in `X_train`.
* (`target_b_idiosyncratic`, …, `target_b_4f_neutral`): the targets that may help you build your models. Target\_w, r, g, b refer to 7, 28, 63, 91 days compounding of returns.&#x20;
* In particular, `target_w`  is a quantised version of the target that you are scored against. Its value is quantised in 7 bins, following the time-dependent, fat-tailed geometry of returns.

### X\_test

Same structure as `X_train` but comprises only 5 Moons. This file is used to simulate the submission process locally via `crunch.test()` (within the code), or `crunch test` (via the cli). The aim is to help participants debug their code and have successful submissions. A successful local test usually means no errors during execution on the submission platform.

## The Performance Metric

The infer function from your code will return your predictions. The latter will be processed in order to compute your performance for each cross-section. We call this performance metric `alpha_score`.

Your inference is [processed](https://github.com/crunchdao/crunch-cli/blob/main/crunch/vendor/datacrunch.py#L18) as follows:

1. **Gaussianisation of the prediction:** This step ensures the standardization of the inferences. Different model and method may give inferences that are calibrated differently. In order to compare them, this step is recentering, rescaling and reshaping all the inferences.
2. **Orthogonalisation:** linear neutralization against industries and common sources of returns. This step makes sure that the information provided by your inference is not too obvious (industry) or already known by DataCrunch; in other words, DataCrunch makes sure that you contribute incrementally to their modeling capabilities.
3. **Mean-zero:** in addition to the neutrality of the prediction, setting it to mean zero informs the score about the dollar neutrality constraint of the portfolio at rebalancing.&#x20;
4. **L1 normalisation:** By keeping the sum of the absolute value constant and mean zero from the previous step, we ensure that your inference is as close as possible to a dollar-neutral, constant size, and tradable portfolio for DataCrunch.
5. **Dot Product:** dot product against the un-quantised version of `target_w`. This product is a proxy for the weekly returns of the simulated portfolio weights multiplied by the return of each individual position.

### Final Score

Your final score will be the cumulative product over time of the above simulated weekly returns (i.e., `alpha_score`) over the the Out-Of-Sample period.  The use of a non-local metric is key in being able to estimate the covariance structure of the various ML-based alphas, beside reflecting the compounding nature of financial returns.

### Interacting with the performance metric

The `alpha score` can be called locally and in the cloud environment with the function `crunch.alpha_score()`. This should be useful for you to cross-validate your model or even use it in your fitness function. In this way, data privacy is preserved and, at the same time, you are provided with the necessary informations to build performant models.

### Reward Scheme

The Cash Prize will be shared as follow:

```python
import numpy as np

payout_pool = 3125 # $CRUNCH
player_score = np.cumprod(alpha_score + 1) - 1
player_payout_factor = player_score if player_score > 0 else 0
player_share = player_payout_factor / sum(players_payout_factor)
player_payout = player_share * payout_pool
```

## Computing Resources

Competitors will be allocated a specified quantity of resources within the cloud environment for the execution of their code automatically. During the submission phase, they are entitled to 10 hours of computing time per week, and for the scoring phase, this allocation increases to 20 hours per week.

## IP Sharing

Only the predictions of the models will be used by DataCrunch, and only for research purposes.

## Quickstarter Notebook

A Quickstarter notebook, together with an Exploratory Data Analysis one, is available below so you can get familiar with what is expected from you and how to use the `crunch.alpha_score()` function.

{% embed url="https://github.com/crunchdao/quickstarters/tree/master/competitions/datacrunch-rally" %}

##
