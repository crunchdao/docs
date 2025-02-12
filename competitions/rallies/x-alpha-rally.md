---
description: >-
  CrunchDAO & X Alpha offer this first of a series of $10,000 Bounty to build
  the most Powerful, Robust and Unbiased AI-Driven VC algorithm.
cover: ../../.gitbook/assets/banner (8).webp
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

# 🔒 X-Alpha Rally

## Overview

The evolving landscape of [Venture Capital](https://en.wikipedia.org/wiki/Venture_capital) is marked by the automation of both data and investment funds, including Micro VC and solo general partners. This trend is leading to a comprehensive decentralization within the venture class. Against this backdrop, investors and Limited Partners are increasingly in need of a systematic approach to maximize returns across diverse asset classes.

In response to this need, the CrunchDAO is crunching the data of more than 2 million startups and 28 million founders, in order to discover the hidden patterns and relationships that will fuel the next wave of venture capitalism.

## Problem Statement

### **Cross-sectional Approach**

The proposed approach allows for a comprehensive analysis of startups by simultaneously examining various data points and trends. This method contrasts with traditional models by integrating diverse data sets and employing advanced statistical techniques to discern both linear and non-linear relationships.

Such a multifaceted view enables more accurate predictions and therefore effective capital allocation, incorporating quantitative risk management strategies not commonly used in the Venture Capital sector.

### **Supervised Classification Approach**

Startups have been categorized, enabling participants to develop supervised learning algorithms. Startups labeled as 1 are expected to achieve higher valuations, while those labeled as 0 are not anticipated to experience significant valuation growth.

## Evaluation

### Objective

The goal of the participant is to perform binary classification \[0, 1] on a universe of startups.

This requires submitting the exact value of 0 or 1 for each investment opportunity (row of the dataset). Startups labeled as 1 are expected to achieve higher valuations, while those labeled as 0 are not anticipated to experience significant valuation growth.

The ensembling of all the submissions will allow identifying which investments are likely to perform better than others by ranking them according to the overall community consensus on each opportunity.

### **Scoring Metric**

The [F1 score](https://en.wikipedia.org/wiki/F-score) will be used in order to assess the models performance effectively. This metric balances the precision (true positives identified by the algorithm) and recall (accounting for missed opportunities). For the algorithm to demonstrate its effectiveness, it must accurately identify investment opportunities while minimizing false negatives and false positives. The F1 score will provide a comprehensive view of the algorithm's accuracy and reliability.

$$
F1=\frac{2∗Precision∗Recall}{Precision+Recall}
$$

Where:

$$
Precision=\frac{TP}{TP+FP}
$$

$$
Recall=\frac{TP}{TP+FN}
$$

## **Competition Phases and Format**

In the first phase, participants are required to submit either a Python notebook (.ipynb) or Python script (.py) file. This file should contain the necessary code to build, load, or update their models trained on the data. The code will be executed by the CrunchDAO on the Out-Of-Sample data. Participants can either submit static models, trained only once on the initial training set, or dynamic models that update or retrain themselves on the unseen data, as explained further in the documentation.

## Timeline

**Submission Phase:**

* **December 8, 2023, 06:00 PM CET** - Start of the competition.
* **January 8, 2023, 05:59 PM CET** - Submission deadline. You must accept the competition rules before this date.

**Out-of-Sample Phase:**

After the final submission deadline, there will be one update to the leaderboard that will reflect your score on the OOS data.

* **January 9, 2023, 06:01 PM CET** - Out-of-Sample scoring begins.
* **January 12, 2023, 06:00 PM CET** - Final Leaderboard release.

## Data

Each row of the dataset describes an investment vehicle at a certain date.

Here follows a concise description of the columns of the files:

`X_train`:

* `date`: A sequentially increasing integer representing a date. Time between subsequent dates is a constant, denoting an unknown but fixed frequency at which the data is sampled. The initial training dataset is composed of 268 dates.
* `id`: A unique identifier representing the investment vehicle at a given date. Note that the same asset has a different `id` at each date.
* `feat_1, ..., feat_n`: Anonymized features describing an investment vehicle at a given date.

`X_test`:

* Same structure as `X_train` but comprises only a few dates. This file is used to simulate the submission process locally via `crunch.test()`, or `crunch test` (if using the CLI). A successful local test usually means no errors during execution on the submission platform.

The dataset is obfuscated.

## Prize

The winners will be determined at the end of the [Out-of-Sample](../../other/glossary.md#out-of-sample-phase) period, based on the metric described in the [#evaluation](x-alpha-rally.md#evaluation "mention") section.

| Winners’ rank | Prize value |
| ------------- | ----------- |
| 1st Place     | $6,000 USD  |
| 2nd Place     | $3,000 USD  |
| 3rd Place     | $1,000 USD  |

## Original Documentation

{% embed url="https://crunchdao-1.gitbook.io/quant-venture-capital-documentation/the-tournament/prize" %}
