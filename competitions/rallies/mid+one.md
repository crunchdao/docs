---
description: Attacking together, make us stronger.
icon: lock
cover: ../../.gitbook/assets/banner (6).webp
coverY: 0
---

# Mid+One

Welcome to Mid+One! Dive into the world of martingales and market dynamics. Spot tiny shifts in high-frequency time-series, to predict where prices are heading. It's all about finding that elusive mid-price, one minute into the future.

Ready to attack?

With Mid+One Crunch has found another opportunity: Thousands of banks are consuming mid-market prices for their execution algorithms. The community meta-model will unlock a stream of ongoing rewards with a potential to serve a multi-billion dollar market.

## TL;DR

{% hint style="info" %}
* Detect small exceptions to the martingale property of a time-series.
* Determine when a time-series will rise or fall.
* A "buy and hold" strategy is applied for each prediction over the next 30 time steps.
* The goal is to maximize profit after accounting for the transaction costs.
* Only one attacker can be selected for OOS and Reward
* [Quickstarter notebook](https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/mean_reversion_attacker/mean_reversion_attacker.ipynb).
{% endhint %}

## Problem Statement

You will build algorithms that takes one data point at a time and decides whether the average future value 30 steps in advance will be higher or lower than the present value. You only have the time-series, nothing else. Your prediction must be determined only by the past history of the time-series and by patterns you detect therein.

Unlike most forecasting tasks, however, you goal is not to provide a precise prediction 30 steps in the future. Instead you should decide between three possibilities:

1. The time-series will go up, on average, by at least `EPSILON`;
2. The time-series will go down, on average, by at least `EPSILON`;
3. Or the average value of the time series will fall between `-EPSILON` and `EPSILON`.

{% hint style="info" %}
`EPSILON` is the Transaction cost value and is set at `0.0025`
{% endhint %}

## Evaluation

For every non-zero prediction, the system initiates a "buy and hold" for 30 data points.

If the prediction is positive we go buy and hold.

If the prediction is negative we go short and hold.

However, a fixed transaction cost (`EPSILON`) is applied to the profit in both case.

#### Example

If the value rises by `0.50` over the next 30 periods; the profit will be `0.50` and the net profit would be `0.4975`.

Similarly, should the price fall by `0.20` then the net profit would be `-0.2025`.&#x20;

### Only one Model on the Leaderboard

In the second Rally, you have to choose which model will appear on the leaderboard.

You can still play with 4 different models.

{% hint style="info" %}
[Learn more how to select your model...](../leaderboard/#only-one-model-on-the-leaderboard)
{% endhint %}

### Time constraints

{% hint style="danger" %}
Your tick and predict must run in less then 20ms!
{% endhint %}

In Mid+One, delivering value to the customer quickly is crucial. Crunch aims to tackle increasingly lower frequencies.&#x20;

As a result, in the infer function, your code must not take more than 20ms from receiving the message to returning the result. If your average inference time exceeds this limit, your position on the leaderboard will be marked with an "Out-Of-Range" badge.

### Out-of-Range badge

Submissions that are too slow but still achieve great results will be rewarded for the Rally.

However, they cannot be deployed in Production or used by Financial Institutions. If you're out of range, there are plenty of ways to optimize your code to meet the 20ms threshold.

{% hint style="info" %}
Keep in mind that gains in Production will be much higher than during the Rally. OPTIMIZE!
{% endhint %}

## Phases and Format

## Timeline

Mid+One is going to evolve into a live Crunch. We went through a first 2 months test phase called "Rally" in order to ensure both problem statement, data and models integrity.&#x20;

* **Friday Oct 18, 2024, 09:00 AM CET** - First rally open
* **Wednesday Dec 18 , 2024, 09:00 AM CET** - First Out-of-Sample scoring
* **Wednesday Jan 8, 2025, 09:00 AM CET** - Submission re-open - Second Rally
* **Sunday Feb 16, 2025, 11:59 PM CET** - Out-of-Sample - Second Rally
* Live is soon to be announced

{% hint style="info" %}
**Live** refers to "in Production" mode where Crunch and the submitted models actively serve real-world end customers.
{% endhint %}

### Submission Phase

During the [Submission Phase](../../other/glossary.md#submission-phase) the Crunchers are required to submit valid Notebooks or Python files. This submission need to "run" successfully on the Crunch hub in order to receive to be triggered in [Out-of-Sample Phase](../../other/glossary.md#out-of-sample-phase) and receive live data.

{% hint style="info" %}
[Get started quickly with a Quickstarter!](https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/mean_reversion_attacker/mean_reversion_attacker.ipynb)
{% endhint %}

### **Out-of-Sample Phase**

Once the [Out-of-Sample Phase](../../other/glossary.md#out-of-sample-phase) start, new data will be run through the models submitted.

## Data

In Mid+One, participants are facing univariate time-series called [Streams](../participate/data.md#stream). Crunch's Streams are iterator objects that allow you to traverse all elements of a time-series, one at a time.

```python
# Print the first x_train time-serie's content
for message in x_train[0]: 
    print(message)

# Would print
# ({x: 10303.346153849048})
# ({x: 10303.500000002896})
# ({x: 10303.461538464431})
# ({x: 10303.461538464431})
...
# ({x: 10303.269230772126})
# ({x: 10303.384615387501})
# ({x: 10303.307692310584})
```

## Building an Attacker with the Mid+One package

This package is intended to make life simpler for those participating in Mid+One.

```bash
$ pip install --upgrade midone
```

{% hint style="info" %}
[Find the Mid+One package's code here.](https://github.com/microprediction/midone)
{% endhint %}

## Some Concepts

* `Attacker` is a Python class that consume a univariate sequence of numerical data points (such as stock prices, bond prices, or any time series) `x1`, `x2`, …`xt` and attempts to predict its future movement.
* `Tick` is a method from the `Attacker` class that allows the consumption of incoming data points.
* `Predict` is a method from the `Attacker` class that take a decision base on previous data points.
* `Tick&Predict` is a method from the `Attacker` class that do `Tick` and `Predict` in a single function call.
* `Accounting` handle tracking and logging the profit and loss (PnL) for decisions made by an `Attacker`.

### Example Attackers

{% hint style="info" %}
Read more in [attacker.md](https://github.com/microprediction/midone/blob/main/midone/attackers/attacker.md).
{% endhint %}

<table><thead><tr><th width="263">Notebook</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://github.com/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/mean_reversion/mean_reversion.ipynb">Mean reversion</a></td><td>A minimalist contest entry notebook</td></tr><tr><td><a href="https://github.com/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/mean_reversion_attacker/mean_reversion_attacker.ipynb">Mean reversion attacker</a></td><td>Illustrates use of the Attacker class</td></tr><tr><td><a href="https://github.com/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/momentum_attacker/momentum_attacker.ipynb">Momentum attacker</a></td><td>Illustrates use of running calculations</td></tr><tr><td><a href="https://github.com/crunchdao/quickstarters/blob/master/competitions/mid-one/quickstarters/regression_attacker/regression_attacker.ipynb">Regression Attacker</a></td><td>Illustrates running regression pattern</td></tr></tbody></table>

### Attackers FAQ

Some common questions have already been answered in the [FAQ.md](https://github.com/microprediction/midone/blob/main/midone/attackers/FAQ.md).

## Prizes

* In the first Rally, the top ten performers judged by profit and loss will share $10,000 in proportion to their profit in the out of sample period. (done)
* In the second Rally, the top 50 performers with positive profit and loss will share $10,000 in proportion to their profit in the out of sample period.

{% hint style="info" %}
[An example is available here.](https://github.com/microprediction/midone/blob/main/PRIZES.md)
{% endhint %}
