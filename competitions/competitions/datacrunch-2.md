---
description: Where Data Meets Trading Edge.
cover: ../../.gitbook/assets/banner (9).webp
coverY: 0
---

# DataCrunch #2

DataCrunch is a market-neutral hedge fund that trades within the Russell 3000 universe. This competition aims to generate accurate weekly prediction signals to support trading decisions in a real portfolio environment.

## Quick TL;DR

* The target is 28 days, or 4 moons.
* The embargo is the size of the target (4 moons).
* The tournament lasts 6 months, with a two-week [Submission Phase](../../other/glossary.md#submission-phase). After that, your models are locked.

## Scoring and Evaluation Method

Two key metrics are used to assess participants:

1. **Pearson Mean Correlation / Pearson Standard Deviation**: Evaluates the consistency and stability of predictive accuracy.\
   This scoring system is used during the Submission Phase and at the beginning of the 6-month [Out-of-Sample Phase](../../other/glossary.md#out-of-sample-phase).
2. **Rolling 6-month Sharpe ratio**: based on simulated portfolio returns derived from weekly predictions.\
   This score is calculated weekly during the six-month out-of-sample phase.

This dual-metric approach rewards both strong predictive capabilities and robust real-world trading performance.

## Data Sources

DataCrunch sources all datasets internally:

* The local data set contains \~15 years of data.\
  A small subset of the last 2.5 years is provided for running experiments on platforms such as Google Colab.
* The public leaderboard contains data from the past 10 months.
* The private out-of-sample dataset contains \~4 years of data and will be used for internal meta-modeling and selection for live deployment.
* The official out-of-sample period contains the six months preceding the data release and will be used to compute the Pearson metric. After the Out-of-Sample phase begins, it will expand by one week each week.

A shorter version of the dataset (2015-2020) will be provided for running experiments on\
platforms like Google Colab.

## Payout structure

Rewards are distributed through two separate prize pools:

* **Pearson Mean Correlation / Pearson Standard Deviation**: $20,000[^1], awarded after the submission  \
  phase.
* **Rolling 6-month Sharpe**: A weekly $1,000[^1] reward, starting from week 2 and running  \
  through the full Out-of-Sample period.

The reward scheme is calculated as follows for both metrics:

{% code title="reward_calculation.py" %}
```python
percentile_rank = your_rank / total_participants  # 0 = worst, 1 = best

if percentile_rank <= 0.5:
    reward = 0
else:
    e = 2 * (percentile_rank - 0.5)  # excess above median, scaled 0–1
    weight = e ** 20

    reward = 20000 * (weight / sum_of_all_eligible_weights)
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

## Execution Timeline

* **Data Release**: December 15, at 6pm UTC
* **Submission window closes**: December 31, at 6pm UTC
* **Out-of-Sample Phase begins**: January 2
* **Out-of-Sample weekly compute**: every Sunday, at 6pm UTC
* **Out-of-Sample weekly leaderboard release**: a week later after compute
* **Out-of-Sample Phase concludes**: End of June

## Resources

### Compute

* **Submission Phase**: 15h/week
* **Start of Out-of-Sample**: 30h
* **Continuous Out-of-Sample**: 10h/week

### Models

Participants can submit a maximum of 4 models.

Teams are not allowed.

## Quickstarter

A Quickstarter Notebook is available so you can get familiar with what is expected from you.

[^1]: In equivalent USD.
