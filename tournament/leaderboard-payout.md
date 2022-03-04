---
description: Leaderboards - Payout description
---

# Leaderboard & Payout

### Round Leaderboard

You can access your weekly [Round Leaderboard here](https://tournament.crunchdao.com/leaderboard).

![Example of Round LB](<../.gitbook/assets/image (20).png>)

Your Round LB score is calculated based on the Mean [Spearman Ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html) correlation between your predictions and the targets.

```python
from scipy.stats import spearmanr

def spearman(y_true, y_pred): 
    return spearmanr(y_pred, y_true).correlation * 100
```

{% hint style="warning" %}
The Round LB scoring is only here to give you feedback on hold out data and will **NOT** be used for payout. Be careful to not overfit on test data...
{% endhint %}

## Global Leaderboard

You can acces the [Global Leaderboard here](https://tournament.crunchdao.com/global-leaderboard/).

![Example of Global Leaderboard](<../.gitbook/assets/image (23) (1).png>)

{% hint style="warning" %}
**Only the Global LB** based on live data **will be used for payout**.
{% endhint %}

The Global Leaderboard score is calculated as follow:

1. Snapshot for payout calculation taken on Thursdays the week before last of the month
2. Mean of last 2 submissions per dataset
3. Mean Correlation Score across all datasets&#x20;

{% hint style="info" %}
Scores are **scaled from `[0; 1]`** based on the top bottom scores.
{% endhint %}

## Payouts

The snapshot for payout calculation is on Thursdays the week before last of the month.

You will be rewarded with our utility $CRUNCH token.

Payout happens after month completion.



## Payouts calculation

Payouts are computed based on global ranks and distributed as an exponential function to the global live leaderboard positions.

![% of the monthly reward distributed by position on the live leaderboard](<../.gitbook/assets/image (23).png>)

This proposal has been done by @correlator and has been voted by the community.

After downloading the live leaderboard as csv, you can run this google Colab to re-compute the payouts and/or propose new payout function:

[https://colab.research.google.com/drive/15ZyqG2wo0bspyD0LTameN8xsRfD4wKe8?authuser=3#scrollTo=2ieUKFqEXq6M](https://colab.research.google.com/drive/15ZyqG2wo0bspyD0LTameN8xsRfD4wKe8?authuser=3#scrollTo=2ieUKFqEXq6M)

