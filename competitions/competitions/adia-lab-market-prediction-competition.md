---
icon: lock
cover: ../../.gitbook/assets/banner (5).webp
coverY: 0
---

# ADIA Lab Market Prediction Competition

## **A cross-section forecast problem** <a href="#the-cross-section-forecast-problem" id="the-cross-section-forecast-problem"></a>

In finance, predicting asset price returns is a fascinating yet very hard problem. For this reason, alternative prediction problems have emerged in an attempt to circumvent these difficulties and still obtain predictions with tradeable potential. One of the most interesting alternatives is the problem of identifying the relative ordering in performance of an investment vehicle, in the cross-section of a pool or subset of them. This is the _cross-section forecast problem_. In this setting, we track a pool of investment vehicles that are generally obtained through some rule (for example S\&P 500 tracks the stock performance of the 500 largest companies in the US) at different dates. This pool is known as the _universe_ in financial jargon and its definition is an object of study by itself. The goal of this competition is to rank the performance of all assets in the universe from best to worst at each given date. The target to predict in this competition is the ranking of the future performance of each asset, remapped to the interval \[-1,1], and the scoring function is Spearman's rank correlation between the predicted vs true rankings.

To illustrate an interesting use case of this problem, we can imagine an investment strategy that is long on the best-performing element of the universe, and short in the worst. In this setting, no matter the direction of the market is still possible to obtain positive returns - or to minimize losses.

The dataset presented to the competitors is an obfuscated version of high-quality market data. Therefore, details such as the nature of each investment vehicle, the constant frequency at which dates are measured, and the definition of each feature, are not available. We hope you enjoy the challenge!

## **Competition Phases and Format**

This competition is focused on forecasting and has two phases. The first is the _submission phase_ where participants can submit and test their models. The second phase, which is automatic, involves running the models against unobserved live market data.

### Submission phase - **12 weeks**

From **May 16, 2023, 05:00 PM CET** to **August 16, 2023, 23:99 PM CET**.

In the first phase, participants are required to submit either a Python notebook (.ipynb) or Python script (.py) file. This file should contain the necessary code to build, load, or update their models trained on the data. The code will be executed by the CrunchDAO platform for every submission, to obtain predictions on unseen data. Participants can either use static models, trained only once on the initial training set, or dynamic models that update or retrain themselves on the unseen data, as explained further in the documentation.

### Out-of-Sample phase - **12 weeks**

From **August 16, 2023, 00:00 PM CET** to **November 16, 2023, 00:00 PM CET**.

In the second phase, also called [Out-of-Sample](https://en.wikipedia.org/wiki/Cross-validation_\(statistics\)) (OOS), the participant's code will be automatically run by the platform on live market data and evaluated. In this phase, the participants won't be able to modify their code.

### Why the two-phase approach?

* Only the performance on Out-of-Sample data will be taken into account.
* Reproducibility of the winning solution is ensured.
* Participants won't be able to exploit data leaks.

{% hint style="info" %}
CrunchDAO is acting as a third-party intermediary in this competition and will never communicate the code to the organizer in any way.
{% endhint %}

## Evaluation

### The objective of the competition

The goal of the participant is to rank the target variable for each stock in the Adia Lab investment universe, from the highest to the lowest, at each given date.

This doesn't require estimating the exact target value for each investment; rather, it involves identifying which investments are likely to perform better than others. Participants can obtain this information from the various features (or Xs) describing each investment at each date in the provided dataset. The features' meanings are unknown to both CrunchDAO and the participants to prevent bias and facilitate sharing of the anonymized dataset.

### The scoring metric

This competition is evaluated on [Spearman Rank Correlation](https://en.wikipedia.org/wiki/Spearman's_rank_correlation_coefficient).&#x20;

Each row in the test set represents the predictions (X) associated with a stock of the universe at a given date and its target (Y).

## Data

Each row of the dataset describes an investment vehicle at a certain date.

Here follows a concise description of the columns of the three files comprising the dataset, `X_train` and `y_train`.

`X_train`:

* `date`: A sequentially increasing integer representing a date. Time between subsequent dates is a constant, denoting an unknown but fixed frequency at which the data is sampled. The initial training dataset is composed of 268 dates.&#x20;
* `id`: A unique identifier representing the investment vehicle at a given date. Note that the same asset has a different `id` at each date.
* `0,...,460`: Anonymized features describing an investment vehicle at a given date. Derived from high-quality market data.

`y_train`:

* `date`: Same as in `X_train`.
* `id`: Same as in `X_train`.
* `y`: The target value to predict. It is related to the future performance of the investment vehicle at the given date. The value is normalized between `-1` and `1`.

`X_test`:

* Same structure as `X_train` but comprises only a few dates. This file is used to simulate the submission process locally via `crunch.test()`, or `cruch test`. The aim is to help participants debug their code and have successful submissions. A successful local test usually means no errors during execution on the submission platform.

The dataset is obfuscated.

## Prize

The winner's rank will be determined at the end of the [Out-of-Sample](../../other/glossary.md#out-of-sample-phase) period, based on the metric described in the [#evaluation](adia-lab-market-prediction-competition.md#evaluation "mention") section.

<table><thead><tr><th width="350"> Winner’s rank</th><th> Prize value</th><th data-hidden></th></tr></thead><tbody><tr><td>1st Place</td><td>$40,000</td><td></td></tr><tr><td>2nd Place</td><td>$20,000</td><td></td></tr><tr><td>3rd Place</td><td>$10,000</td><td></td></tr><tr><td>4th Place</td><td>$5,000</td><td></td></tr><tr><td>5th Place</td><td>$5,000</td><td></td></tr><tr><td>6th Place</td><td>$5,000</td><td></td></tr><tr><td>7th Place</td><td>$5,000</td><td></td></tr><tr><td>8th Place</td><td>$3,500</td><td></td></tr><tr><td>9th Place</td><td>$3,500</td><td></td></tr><tr><td>10th Place</td><td>$3,000</td><td></td></tr></tbody></table>

## Original Documentation

{% embed url="https://docs.adialab.crunchdao.com/" %}
