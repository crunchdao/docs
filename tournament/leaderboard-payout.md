---
description: Leaderboards - Payout description
---

# 🏎 Leaderboard - Payout

### Round Leaderboard

You can access your weekly Round[ Leaderboard here](https://tournament.crunchdao.com/leaderboard)

![Example of Round LB](<../.gitbook/assets/image (20).png>)

Your Round LB score is calculated based on the Mean [Spearman Ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html) correlation between your predictions and the targets.

```
// from scipy.stats import spearmanr
def spearman(y_true, y_pred): 
    return spearmanr(y_pred, y_true).correlation * 100
```

{% hint style="warning" %}
The Round LB scoring is only here to give you feedback on hold out data and will **NOT** be used for payout. Be careful to not overfit on test data...
{% endhint %}

## Global Leaderboard

You can acces the [Global Leaderboard here](https://tournament.crunchdao.com/global-leaderboard/)

![Example of Global Leaderboard](<../.gitbook/assets/image (23).png>)

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
