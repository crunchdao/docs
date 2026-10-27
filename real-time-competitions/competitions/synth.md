---
description: >-
  Forecast the price changes of crypto assets, gold, oil, the S&P 500 and
  tokenized stocks as probability distributions, not single values.
---

# Synth: Synthetic Price Data

## Overview

[Synth](https://www.synthdata.co/) is a decentralized prediction subnet on Bittensor where miners forecast asset prices as probability distributions, which captures volatility and tail risks that single-value forecasts miss.

In this Crunch, you build models, called **trackers**, that forecast the distribution of an asset's price changes over two **horizons**: the next 1-hour and the next 24-hours. The best trackers are combined into an ensemble, and that ensemble is what Crunch mines on Synth. Trackers are built using the [crunch-synth](https://github.com/crunchdao/crunch-synth) package.

### Latest Changes

* **Synth score**: three new informational leaderboard columns (only for 24-hour horizon for readability reasons). See [Leaderboard](synth.md#game-state).
* **Synth score timing**: each Synth prompt is now answered with the latest round made before its request, as our miner does. See Leaderboard.
* **Volatility on 1 hour**: the 1-hour score now includes Synth's volatility CRPS. See [Scoring](synth.md#scoring).
* **Rewards paused**: Crunch's miners were deregistered from Synth. We'll register new ones once the community models do better on the 24-hour horizon. See [Payouts](synth.md#payouts).
* **`crunch-synth` 0.15.0**: local scores now match the leaderboard exactly, and `evaluate_synth_score` gives you your Synth score locally. The quickstarters were updated too.
* **Rounds**: prediction rounds now run every 39 minutes (24 hours) and every 8 minutes (1 hour) per asset.

### Covered assets

| Asset                    | Ticker   | Horizons |
| ------------------------ | -------- | -------- |
| Bitcoin                  | `BTC`    | 1h + 24h |
| Ethereum                 | `ETH`    | 1h + 24h |
| Solana                   | `SOL`    | 1h + 24h |
| Ripple                   | `XRP`    | 1h + 24h |
| Hyperliquid              | `HYPE`   | 1h + 24h |
| Tether Gold              | `XAUT`   | 24h      |
| S\&P 500 Index           | `SP500`  | 24h      |
| NVIDIA tokenized stock   | `NVDAX`  | 24h      |
| Tesla tokenized stock    | `TSLAX`  | 24h      |
| Apple tokenized stock    | `AAPLX`  | 24h      |
| Alphabet tokenized stock | `GOOGLX` | 24h      |
| SpaceX tokenized stock   | `SPCX`   | 24h      |
| Crude Oil WTI            | `WTIOIL` | 24h      |

### Phases

* ~~Phase 1: 1-month warm-up before live trading~~ (completed)
* ~~Phase 2: 3 months live with a fixed $30,000 USDC prize pool~~ (completed)
* **Phase 3: rewards from mining the Synth subnet (current)**, see [Payouts](synth.md#payouts)

## What you predict

For a step of $$k$$ seconds (5 minutes, 1 hour, …), your tracker predicts the **price change** over that step:

$$
r_{t,k} = P_t - P_{t-k}
$$

The change is in the asset's price units, not a percentage. Rather than one value, your tracker returns a full **probability density function (PDF)** of $$r_{t,k}$$ for every step up to the horizon. Below is an example of a density forecast of price changes over the next 24 hours, at 5-minute steps:

<figure><img src="https://3895192033-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FrZv71uKEauXe2J8CJm1h%2Fuploads%2FnuN5AApOG0aVraNkYQG4%2Fimage.png?alt=media&#x26;token=851f03af-035a-41f4-8da4-6fbaf80a130a" alt=""><figcaption></figcaption></figure>

In code, a tracker asked for `SOL` over 24 hours at 5-minute steps returns one density per step:

```python
>>> model.predict(asset="SOL", horizon=86400, step=300)
[
    {
        "step": (k + 1) * step,
        "type": "builtin",
        "name": "norm",
        "params": {
            "loc": -0.01,   # mean price change over the step
            "scale": 0.4    # standard deviation of the price change
        }
    }
    for k in range(0, horizon // step)
]
```

Mapped back into price space, the same forecast looks like this:

<figure><img src="https://3895192033-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FrZv71uKEauXe2J8CJm1h%2Fuploads%2F7QI7AU2BNdCOUXtK2x9c%2Fimage.png?alt=media&#x26;token=a7805716-147a-47b4-9129-67d527b50588" alt=""><figcaption></figcaption></figure>

## Game Rules <a href="#game-state" id="game-state"></a>

### Prediction rounds <a href="#prediction-phase" id="prediction-phase"></a>

A **prediction round** asks your tracker for one asset and one horizon, at every step resolution of that horizon:

| Horizon  | A round every        | Step resolutions                     | Assets                             |
| -------- | -------------------- | ------------------------------------ | ---------------------------------- |
| 24 hours | 39 minutes per asset | 5 minutes, 1 hour, 6 hours, 24 hours | all 13 assets                      |
| 1 hour   | 8 minutes per asset  | 1, 5, 15 and 30 minutes, 1 hour      | `BTC`, `ETH`, `SOL`, `XRP`, `HYPE` |

* Your tracker has **40 seconds** to return all the densities of a round: **don't submit models that need more!**
* Each density can be a single distribution or a **mixture of at most 3 components**.
* A prediction that is late, malformed or has more components counts as missing.
* Each player can run **two active models** and update them at any time.
* Players can join or leave the game whenever they want.
* Since the ranking uses a 7-day average, a new model needs **7 days** to get a meaningful rank.
* A model whose Recent, Steady **and** Anchor scores (see [Leaderboard](synth.md#game-state)) are all below **0.30** is disabled automatically. It cannot be disabled again in the **72 hours** that follow, which gives a resubmitted model time to bring at least one of its scores back above 0.30.

### Scoring <a href="#scoring" id="scoring"></a>

Once its horizon has passed, each prediction is scored with the [**CRPS**](https://en.wikipedia.org/wiki/Scoring_rule) (Continuous Ranked Probability Score) against the final (settled) 1-minute prices, as Synth's validator does, about 2 to 3 minutes after the horizon ends. A lower CRPS means a more accurate forecast. Missing or invalid predictions receive the **worst CRPS of the round**.

On the **1-hour horizon**, the score also includes a **volatility CRPS**, as on [Synth](https://synthdata.co/research/introducing-volatility-crps-updated):

* 1,000 price paths are simulated from your densities.
* Their realized volatility (standard deviation of 1-minute changes) is scored with CRPS over the **full hour**, **four 15-minute blocks** and **twelve 5-minute blocks**, weighted with λ = 5.25: λ/3 for the hour, λ/12 per 15-minute block, λ/36 per 5-minute block.
* This term is usually 1 to 3% of the 1-hour score. A prediction whose densities cannot be turned into price paths counts as invalid.

To compare trackers, the CRPS of each round is turned into a **relative score**:

* The best CRPS of the round gets **1**.
* The worst 5% get **0**.
* The others are scaled linearly in between.

### Leaderboard <a href="#game-state" id="game-state"></a>

The leaderboard averages the relative scores over three rolling windows, across all assets and horizons:

| Columns                                  | Window              | Used for                  |
| ---------------------------------------- | ------------------- | ------------------------- |
| **Anchor**                               | 7 days              | **Ranking and rewards**   |
| Steady, Recent                           | 3 days, 24 hours    | Information               |
| Vol Anchor, Vol Steady, Vol Recent       | 7 days, 3 days, 24h | Information (1-hour only) |
| Synth Anchor, Synth Steady, Synth Recent | 7 days, 3 days, 24h | Information               |
|                                          |                     |                           |

The **Vol** columns show the volatility CRPS on its own. It is already part of the 1-hour scores, so it is not counted twice.

The **Synth** columns show how your tracker would do **on Synth itself**. Each Synth prompt is answered with the latest round made before its request, as our miner does: 1,000 price paths are built from your densities and scored with Synth's own CRPS (including the volatility term on 1 hour). Your gap to the best Synth miner on each prompt is averaged over the window and mapped to a score between 0 and 1:

* **1** means being the best Synth miner on every prompt. Nobody gets there: Synth's top miners currently score about 0.4 on 1 hour and 0.5 to 0.6 on 24 hours.
* A missed round counts as Synth's 95th-percentile result for that prompt, as on Synth.\
  A model that answered less than 80% of the rounds of a window gets no Synth score for it.
* Rounds run on their own schedule, so the round sent to a prompt is on average about 5 minutes old on 1 hour and 22 minutes old on 24 hours.
* The score is available about 35 minutes (1 hour) or 3 hours (24 hours) after a round resolves, once Synth has published its own scores.
* It is **not used for ranking or rewards**: it shows how far the ensemble is from what Synth rewards.

The `synth/benchmarktracker` row replays a real Synth miner, Synth's **#1** on the 1-hour horizon and its **#20** on the 24-hour horizon. It is there for comparison and never receives rewards.

### Payouts <a href="#payouts" id="payouts"></a>

{% hint style="warning" %}
**Rewards are currently paused.** Crunch's miners were deregistered from the Synth subnet in October 2026. New miners will be registered once the community models perform well enough on the 24-hour horizon (see the Synth score). Until then, there are no mining rewards to distribute.
{% endhint %}

Rewards come from the emissions of the [Synth miners run by Crunch](https://taostats.io/subnets/50/metagraph?order=stake%3Adesc\&filter=5ECfazM69yvhX4SLX2CDzW4iSJvzJx3XESkhdncqM3HmyfKf), and are paid every week, one day after the end of each 7-day period. Only the new emissions of the period count; the stake itself is not withdrawn.

* A model hosting fee of 500 USD per month ([115.07 USD per week](#user-content-fn-1)[^1]) is deducted.
* A 20% platform fee is deducted from the rest.
* What remains is **the Pot**, converted at the creation of the checkpoint from [Alpha to TAO](#user-content-fn-2)[^2] and from [TAO to USD](#user-content-fn-3)[^3].

The Pot is shared by at most 8 players:

* The top 4 of the 1-hour horizon.
* The top 4 of the 24-hour horizon.
* A player in both receives the sum of both prizes.

## Probabilistic Forecasting

Probabilistic forecasting provides **a distribution of possible future values** rather than a single point estimate. Instead of predicting only the most likely outcome, it estimates the range of potential outcomes and their probabilities.

Formally, a probabilistic forecast models the conditional distribution of a future value $$Y_t$$ given the past observations $$(\mathcal{H}_{t-1})$$:

$$
P(Y_t \mid \mathcal{H}_{t-1})
$$

The model estimates a full distribution $$(f(Y_t \mid \mathcal{H}_{t-1}))$$ instead of a single prediction $$(\hat{Y}_t)$$. It can take different parametric forms, such as a Gaussian:

$$
Y_t \mid \mathcal{H}_{t-1} \sim \mathcal{N}(\mu_t, \sigma_t^2)
$$

where $$(\mu_t)$$ is the predicted mean and $$(\sigma_t)$$ the uncertainty of the forecast.

A **mixture density**, such as the Gaussian mixture $$\sum_{i=1}^{K} w_i \mathcal{N}(Y_t | \mu_i, \sigma_i^2)$$, can capture multi-modal and fat-tailed distributions. Here is a mixture of two normal distributions, in the format your tracker returns (up to 3 components):

```python
{
    "step": step,
    "type": "mixture",
    "components": [
        {"density": {"type": "builtin", "name": "norm", "params": {"loc": mu_1, "scale": sigma_1}}, "weight": 0.7},
        {"density": {"type": "builtin", "name": "norm", "params": {"loc": mu_2, "scale": sigma_2}}, "weight": 0.3},
    ]
}
```

<figure><img src="https://3895192033-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FrZv71uKEauXe2J8CJm1h%2Fuploads%2FGjhqvyCkZ3c6Id5xaN1v%2Fimage.png?alt=media&#x26;token=bf4999ef-e71a-4a90-9f1b-a8a621bb4f48" alt=""><figcaption></figcaption></figure>

The format follows the [density\_pdf](https://github.com/microprediction/densitypdf/blob/main/densitypdf/__init__.py) specification, and [validatedensitydict](https://github.com/microprediction/density/blob/main/density/validatedensitydict.py) tells you whether a density is valid.

## Create your Tracker

A tracker receives asset prices as they arrive and is asked for forecasts at specific times. You can use the prices provided by the challenge or any other dataset to improve your predictions.

### Your tracker

Subclass `TrackerBase` and implement `predict()`. The base class stores the prices for you and calls `predict()` once per step resolution of a round:

```python
import numpy as np
from crunch_synth import TrackerBase

# This is just an example!
class MyTracker(TrackerBase):

    def predict(self, asset: str, horizon: int, step: int):
        # Prices pushed by Crunch, resampled at 5 minutes over the last 3 days
        prices = self.prices.get_prices(asset, days=3, resolution=300)
        if len(prices) < 2:
            return []

        _, values = zip(*prices)
        changes = np.diff(values)  # 5-minute price changes
        ratio = step / 300         # from 5-minute changes to `step`-second changes

        # One density of the price change per step, up to the horizon
        return [
            {
                "step": (k + 1) * step,
                "type": "builtin",
                "name": "norm",
                "params": {
                    "loc": float(np.mean(changes) * ratio),
                    "scale": float(np.std(changes) * np.sqrt(ratio)),
                },
            }
            for k in range(horizon // step)
        ]
```

A few things to note:

* On the **1-hour horizon**, the volatility CRPS rewards a well-calibrated **per-minute volatility** and **consistent scales across step resolutions**, not just a good end-of-hour distribution.
* On the **24-hour horizon**, your step resolutions (5 minutes, 1 hour, 6 hours, 24 hours) must be **consistent with each other**, since Synth also scores the 30-minute, 3-hour and 24-hour changes along the paths.

### Evaluate locally

`TrackerEvaluator` scores your tracker exactly like the live leaderboard (since `crunch-synth` 0.15.0, including the volatility term on 1 hour) and ranks it against the live scores of all trackers over a recent week. Results from older versions of the package are not comparable.

### Check your Synth score

`evaluate_synth_score` replays real Synth prompts, with your tracker predicting at each prompt's request time. With no round delay, it reads slightly higher than the leaderboard:

```python
from datetime import datetime, timezone
from crunch_synth import evaluate_synth_score

result = evaluate_synth_score(
    tracker=MyTracker(),
    asset="BTC",
    horizon=86400,
    steps=[300, 3600, 21600, 86400],
    start=datetime(2026, 10, 5, tzinfo=timezone.utc),
    end=datetime(2026, 10, 10, tzinfo=timezone.utc),
)

print(result)  # SynthScoreResult(BTC 24h: score=0.019, mean gap=168.2, rank #247 of 248 on 30 prompts)
```

{% hint style="info" %}
The [tracker examples](https://github.com/crunchdao/crunch-synth/tree/master/crunch_synth/examples) show a complete tracker, its local evaluation, and the Synth score and volatility CRPS sections.
{% endhint %}

### Price data

The prices your tracker receives come from **PriceDB**, Crunch's price database, which is also the source used to score your predictions. You can query it directly to build your training data:

```python
from datetime import datetime, timezone
from crunch_synth import pricedb

start = datetime(2026, 10, 1, tzinfo=timezone.utc)
end = datetime(2026, 10, 2, tzinfo=timezone.utc)

# 1-minute closing prices, as [(timestamp, price), ...]
prices = pricedb.get_price_history(asset="BTC", from_=start, to=end)

# Or several columns at once, as a pandas DataFrame
candles = pricedb.get_price_history(
    asset="BTC", from_=start, to=end,
    columns=["open", "high", "low", "close", "volume"],
)
```

* Prices are **1-minute bars** only, refreshed every 30 seconds so each bar is stored as soon as it closes. Available columns are `open`, `high`, `low`, `close`, `volume` and `number_of_trades`.
* Every covered asset is available.
* The same data is available over HTTP. See the [API Doc](https://pricedb.crunchdao.com/docs).

## Additional Resources

* [Literature](https://github.com/crunchdao/crunch-synth/blob/master/LITERATURE.md)
* Useful Python [packages](https://github.com/crunchdao/crunch-synth/blob/master/PACKAGES.md)
* Synth: [Introducing Volatility CRPS](https://synthdata.co/research/introducing-volatility-crps-updated)

[^1]: Equivalent to 500 / 4.34524, see why [here](https://www.cuemath.com/questions/what-is-the-average-number-of-weeks-in-a-month/).

[^2]: Conversion rate is coming from taostats' API.

[^3]: Conversion rate is coming from Coingecko's API.
