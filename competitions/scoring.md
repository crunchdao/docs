---
description: How is a prediction scored?
hidden: true
---

# Scoring

## Checking

Before scoring, the prediction dataframe is first checked against the [example prediction](data.md#the-example-prediction).

Different checks are [available](https://github.com/crunchdao/crunch-cli/blob/master/crunch/checker/functions.py) and are activated depending on the crunch:

* Checking column names
* Checking nans and infinites
* Checking if values are in a range
* Checking if values are in an allow list
* Checking the correct keys
* Checking the correct rows
* Checking constants

{% hint style="info" %}
The checks are also performed locally when `crunch.test()` is called, so you get immediate feedback.
{% endhint %}

## Scoring

The prediction is first merged with `y_test` and then divided by each target column.

Each target has one or more metrics that contain all the information such as the scoring function, the reducing function, the unit, the multiplier, ...

### Scoring Function

The functions are called on a subset of the dataframe, allowing for a plotable graph.

<table><thead><tr><th width="202">Name</th><th width="547">Backend</th></tr></thead><tbody><tr><td>Balanced Accuracy</td><td><code>sklearn.metrics.balanced_accuracy_score</code></td></tr><tr><td>Dot Product</td><td><code>numpy.dot</code></td></tr><tr><td>F Beta</td><td><code>sklearn.metrics.fbeta_score</code></td></tr><tr><td>Spearman</td><td><code>pandas.DataFrame.corr</code></td></tr></tbody></table>

{% hint style="info" %}
[Implementations are publicly available.](https://github.com/crunchdao/crunch-cli/blob/master/crunch/scoring/scorers.py)
{% endhint %}

### Reducer Function

Once all the subset of the dataframe has been scored, the values are then reduced to a singular value.

<table><thead><tr><th width="201">Name</th><th>Backend</th></tr></thead><tbody><tr><td>None</td><td>The entire dataframe is scored; the final score is not a reduced value of individual groups.</td></tr><tr><td>Sum</td><td><code>builtins.sum</code></td></tr><tr><td>Mean / Average</td><td><code>statistics.mean</code></td></tr></tbody></table>

{% hint style="info" %}
[Implementations are publicly available.](https://github.com/crunchdao/crunch-cli/blob/master/crunch/scoring/reducers.py)
{% endhint %}

## Split Keys

When data is imported for a competition, it is first pre-processed before it can be downloaded by participants.

The system uses "split keys" internally:

<table><thead><tr><th width="225">Competition Format</th><th>Meaning</th></tr></thead><tbody><tr><td>Time-series</td><td>Moons / regular time interval</td></tr><tr><td>DAG</td><td>Datasets</td></tr><tr><td>Stream</td><td>Time grouped by minutes</td></tr></tbody></table>

These keys work differently depending on how they are configured:

<table><thead><tr><th width="126">Group</th><th width="120">Reduced</th><th>Behavior</th></tr></thead><tbody><tr><td>Train</td><td></td><td>The data is downloadable and must be used for training purposes.</td></tr><tr><td>Test</td><td><code>XY</code> or <code>X</code></td><td>The data is downloadable and must be used for local inference.</td></tr><tr><td>Test</td><td><code>None</code></td><td>The data is not downloadable and is only used for inference in the cloud environment.</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

### Why is test divided in two?

To avoid overfitting, the reduced data is not used for scoring.

The groups that contain reduced keys are skipped when the reducer function is invoked.
