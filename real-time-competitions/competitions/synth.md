---
description: >-
  Forecast prices of crypto assets (Bitcoin, Ethereum, Solana, Ripple,
  Hyperliquid), Tether Gold, crude oil, the S&P 500 and tokenized stocks (Apple,
  Nvidia, Tesla, Alphabet, SPCX).
---

# Synth: Synthetic Price Data

## Overview

[Synth](https://www.synthdata.co/) is a decentralized prediction subnet on Bittensor that forecasts crypto and financial asset prices through probabilistic modeling. Instead of predicting a single price, participants build models that output probability distributions, capturing not just where an asset might go, but the range of possibilities and their likelihoods. This approach helps model real market behavior like volatility clustering and tail risks that traditional forecasts often miss.

In this Crunch, you need to build probabilistic forecasting models for the assets listed below. Your models will predict over 1-hour and 24-hour price distributions scored on statistical accuracy. The best-performing models will be aggregated into an ensemble forecast that captures collective intelligence from our community, and that ensemble is mined on Synth.

## How to participate

Enter Synth and submit full return density forecasts for selected assets.

**Trackers (models)** must generate returns density predictions, not single predictions and maximize accuracy overall forecasts. See open-source Crunch framewor&#x6B;**:** [crunch-synth](https://github.com/crunchdao/crunch-synth)

**Covered Assets:**

* Bitcoin (BTC)
* Ethereum (ETH)
* Solana (SOL)
* Tether Gold (XAUT)
* S\&P 500 Index (SP500)
* NVIDIA tokenized stock (NVDAX)
* Tesla tokenized stock (TSLAX)
* Apple tokenized stock (AAPLX)
* Alphabet tokenized stock (GOOGLX)
* SpaceX tokenized stock (SPCX)
* Ripple (XRP)
* Hyperliquid (HYPE)
* Crude Oil WTI (WTIOIL)

## Phases

* ~~Phase 1: 1-month warmup period before Live Trading~~ (completed)
* ~~Phase 2: 3 months live with a fixed $30,000 USDC prize pool~~ (completed)
* Phase 3: Rewards pools will be generated from mining Synth subnet and may be higher. (current)

## Prediction Targets

Trackers must predict the **probability distribution of returns**, defined as:

$$
r_{t,k} = P_t - P_{t-k}
$$

For each defined step $$k$$ (e.g., 5 minutes, 1 hour, …), your tracker must return a full **probability density function (PDF)** over the future price change $$r_{t,k}$$.

## Visualize the challenge

The Synth coordinator is evaluated on **incremental return predictions**, not raw prices.\
Incremental returns are price changes over each step, in the asset's price units (not percentages), and produce a series that is easier to model than raw prices.

```diff
model.predict(asset="SOL", horizon=86400, step=300)
```

Below is an example of a **density forecast over incremental returns for the next 24h at 5-minute intervals**:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Below is a minimal example showing what your tracker might return:

```python
# Expected output
>>> model.predict(asset="SOL", horizon=86400, step=300)
[
    {
        "step": (k + 1) * step,
        "type": "builtin",
        "name": "norm",
        "params": {
            "loc": -0.01,       # mean price change over the step
            "scale": 0.4        # standard deviation of the price change
        }
    }
    for k in range(0, horizon // step)
]
```

Here is the **return forecast mapped into price space**:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Game Rules <a href="#game-rules" id="game-rules"></a>

#### Start <a href="#start" id="start"></a>

* The game begins with a **model calibration /** **warm-up phase of 4 weeks,** where predictions are scored but **not rewarded.**
* Leaderboard is based on **7-day rolling average** of CRPS scores.
* A player must wait **7 days** to get a meaningful ranking.
* Players may **enter or exit the game at any time**.
* Each player may run **two active models**, which can be updated at any time.

#### Prediction Phase <a href="#prediction-phase" id="prediction-phase"></a>

In each prediction round, players must submit **a set of** **density forecasts**.

A **prediction round** is defined by **one asset**, **one forecast horizon** and **one or more step resolutions**.

* A **24-hour horizon** forecast&#x20;
  * Triggered **every 39 minutes** for each asset
  * Step resolutions: **5-minute, 1-hour, 6-hour and 24-hour**
  * Supported assets: `BTC`, `SOL`, `ETH`, `XAUT`, `SP500`, `NVDAX`, `TSLAX`, `AAPLX`, `GOOGLX`, `XRP`, `HYPE`, `WTIOIL`, and `SPCX`
* A **1-hour horizon** forecast&#x20;
  * Triggered **every 8 minutes** for each asset
  * Step resolutions: **1-minute, 5-minute, 15-minute, 30-minute, and 1-hour**
  * Supported assets: `BTC`, `SOL`, `ETH`, `XRP`, and `HYPE`

All required forecasts for a prediction round must be generated **within 40 seconds**.

{% hint style="info" %}
Don't submit models that need more than 40 seconds to infer!
{% endhint %}

Each density can be a single distribution or a **mixture of at most 3 components**. Predictions with more components are rejected (and count as invalid).

#### Scoring <a href="#scoring" id="scoring"></a>

* Once the full horizon has passed, each prediction is scored using a [**CRPS**](https://en.wikipedia.org/wiki/Scoring_rule) **scoring function**.
* Scoring uses **final (settled) 1-minute prices**, as Synth's validator does. A prediction is scored about 2–3 minutes after its horizon ends.
* **1-hour horizon only (BTC, ETH, SOL, XRP, HYPE): volatility CRPS.** The 1h score is `price CRPS + volatility CRPS`:
  * 1,000 price paths are simulated from your submitted densities.
  * For each path, realized volatility (standard deviation of 1-minute returns) is computed over the **full hour**, **four 15-minute blocks** and **twelve 5-minute blocks**.
  * Each block is scored with CRPS against the realized volatility. Blocks are weighted with λ = 5.25 (λ/3 for the hour, λ/12 per 15-minute block, λ/36 per 5-minute block), matching [Synth](https://synthdata.co/research/introducing-volatility-crps-updated).
  * The volatility term typically accounts for about 1–3% of the 1h score.
* A lower **CRPS score** reflects more accurate predictions.
* Missing or invalid predictions receive the **worst CRPS score of the round**. On the 1-hour horizon this includes predictions whose densities cannot be turned into price paths for volatility scoring.
* **Low-score models are disabled:** a model whose Recent, Steady **and** Anchor scores are all below **0.30** is stopped automatically. After you resubmit it, it has **72 hours** to get back above 0.30 before it can be disabled again.

Leaderboard ranking is based on a **7-day rolling average** of CRPS scores across **all assets and horizons**, evaluated **relative to other participants**, for each prediction round:

* The **best CRPS score is assigned a normalized score of 1**
* The **worst 5% of CRPS scores are assigned** **a score of 0**
* Intermediate scores are scaled accordingly

### Leaderboard & Game State <a href="#game-state" id="game-state"></a>

The leaderboard displays several **relative performance indicators**, computed across **all assets and horizons**:

* **Primary Metric**
  * **Anchor CRPS (7 days):** Rolling relative CRPS average over the past 7 days.
* **Secondary Metrics**
  * **Steady CRPS (3 days):** Rolling relative CRPS average over the past 3 days.
  * **Recent CRPS (24 hours):** Rolling relative CRPS average over the past 24 hours.
* **Volatility Metrics (1-hour horizon, informational)**
  * **Vol Anchor (7 days)**, **Vol Steady (3 days)**, **Vol Recent (24 hours):** Rolling relative volatility CRPS on its own.
  * Volatility is **already included** in the 1-hour CRPS scores above. These columns are not counted a second time in the ranking.
* **Synth Score (informational)**
  * **Synth Anchor (7 days)**, **Synth Steady (3 days)**, **Synth Recent (24 hours)**, per horizon and overall.
  * Measures how your tracker would do **on Synth itself**: each round is matched with the first Synth prompt that starts after it, 1,000 price paths are built from your densities (like our miner does), and they are scored with **Synth's own CRPS** (plus the volatility term on 1h). Your gap to the **best Synth miner** on that prompt is averaged over the window and mapped to a 0–1 score.
  * **1** = best of all Synth miners on every prompt (nobody gets there). Synth's top miners currently score about 0.4 on 1h and 0.5–0.6 on 24h.
  * Missed rounds count as a poor answer: Synth's 95th-percentile result for that prompt.
  * Available about 35 minutes (1h) or about 3 hours (24h) after a round resolves, once Synth has published its own scores.
  * **Not used for ranking or rewards.** It shows how far the ensemble is from what Synth rewards.
* **Benchmark**
  * The `synth/benchmarktracker` row replays a real Synth miner: Synth's **#1** miner on the 1-hour horizon and its **#20** on the 24-hour horizon. It is shown for comparison and never receives rewards.

#### Payouts <a href="#payouts" id="payouts"></a>

* Rewards are distributed at target resolution + 24h (every 7 + 1 days).
* Real mining rewards from Synth Miners.
  * **Currently paused:** see below.
  * A 500 USD/month [(or 115.07 USD/week)](#user-content-fn-1)[^1] model hosting fee is applied.
  * A 20% platform fee (after hosting fee) is applied.
  * **The rest is the Pot.**
  * Price is determined at the creation of the checkpoint by converting [Alpha to TAO](#user-content-fn-2)[^2] and then converting [TAO to USD](#user-content-fn-3)[^3].
* Maximum of 8 players **receive 100% of the Pot.**
  * Top 4 of the 1-hour horizon.
  * Top 4 of the 24-hour horizon.
  * If a participant appears on both horizons, their prize is the sum.
* The miners that Crunch manages can be found [here](https://taostats.io/subnets/50/metagraph?order=stake%3Adesc\&filter=5ECfazM69yvhX4SLX2CDzW4iSJvzJx3XESkhdncqM3HmyfKf).
  * The stake is not withdrawn each week; but only the new emissions are taken into account for the checkpoint.

{% hint style="warning" %}
**Why are the rewards currently paused?**

Crunch's miners were deregistered from the Synth subnet in October 2026. New miners will be registered once the community models perform well enough on the 24-hour horizon (see the Synth Score). Until then, there are no mining rewards to distribute.
{% endhint %}

## Probabilistic Forecasting

Probabilistic forecasting provides **a distribution of possible future values** rather than a single point estimate, allowing for uncertainty quantification. Instead of predicting only the most likely outcome, it estimates a range of potential outcomes along with their probabilities by outputting a **probability distribution**.

A probabilistic forecast models the conditional probability distribution of a future value $$(Y_t)$$ given past observations $$(\mathcal{H}_{t-1})$$. This can be expressed as:

$$
P(Y_t \mid \mathcal{H}_{t-1})
$$

where $$(\mathcal{H}_{t-1})$$ represents the historical data up to time $$(t-1)$$. Instead of a single prediction $$(\hat{Y}_t)$$, the model estimates a full probability distribution $$(f(Y_t \mid \mathcal{H}{t-1}))$$, which can take different parametric forms, such as a Gaussian:

$$
Y_t \mid \mathcal{H}_{t-1} \sim \mathcal{N}(\mu_t, \sigma_t^2)
$$

where $$(\mu_t)$$ is the predicted mean and $$(\sigma_t^2)$$ represents the uncertainty in the forecast.

Probabilistic forecasting can be handled through various approaches, including **variance forecasters**, **quantile forecasters**, **interval forecasters** or **distribution forecasters**, each capturing uncertainty differently.

For example, you can try to forecast the target location by a gaussian density function (or a mixture), thus the model output follows the form:

```python
{
    "density": {
        "type": "builtin",
        "name": "norm",
        "params": {
            "loc": y_mean,
            "scale": y_std
        }
    },
    "weight": weight
}
```

A **mixture density**, such as the gaussian mixture $$\sum_{i=1}^{K} w_i \mathcal{N}(Y_t | \mu_i, \sigma_i^2)$$ allows for capturing multi-modal distributions and approximate more complex distributions.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

The meaning of probabilistic forecast is made more precise by means of the Python [density](https://github.com/microprediction/density) package which provides a dict specification of continuous univariate density function mixtures using the pydantic Python package. The function [validatedensitydict](https://github.com/microprediction/density/blob/main/density/validatedensitydict.py) will tell you whether or not your specification is valid.

## Create your Tracker

A **tracker** is a model that processes real-time asset data to **predict future price changes**. It uses past prices to generate a **probabilistic forecast** of incremental returns. **You can use the data provided by the challenge or any other datasets to improve your predictions.**

It operates incrementally: prices are pushed to the tracker as they arrive and predictions are requested at specific times by the framework.

**To create your tracker, you need to define a class that implements the `TrackerBase` interface, which already handles:**

* price storage and alignment via `PriceStore`
* multi-resolution forecasting through `predict_all()`

As a participant, you only need to implement **one method**: `predict()`.

**Required method: `predict(self, asset: str, horizon: int, step: int)`**

It must return a sequence of **predictive density distributions** for the **incremental price change** of an asset:

* Forecast horizon: horizon seconds into the future
* Temporal resolution: one density every step seconds
* Output length: `horizon // step`

Each density prediction must comply with the [density\_pdf](https://github.com/microprediction/densitypdf/blob/main/densitypdf/__init__.py) specification.

**For 1-hour horizon:** price paths are simulated from your densities at every step resolution and then combined. Volatility CRPS therefore rewards well-calibrated **per-minute volatility** and **consistent scales across step resolutions**, not just a good end-of-hour distribution.

**Evaluate locally:** `TrackerEvaluator` scores your tracker exactly like the live leaderboard (since `crunch-synth` 0.15.0, including the volatility term on 1h) and ranks it against the live scores of all trackers over a recent week. Results from older versions of the package are not comparable.

**Check your Synth Score:** `evaluate_synth_score(tracker, asset, horizon, steps, start, end)` replays real Synth prompts the way our miner answers them and returns your Synth Score, your gap to the best Synth miner and your rank among Synth miners. On 24h, a good score needs your step resolutions (5 minutes, 1 hour, 6 hours, 24 hours) to be **consistent with each other**, since Synth also scores 30-minute, 3-hour and 24-hour changes along the paths.

{% hint style="info" %}
You can refer to the [Tracker examples](https://github.com/crunchdao/crunch-synth/tree/master/crunch_synth/examples) for guidance.\
Both notebooks include a Synth Score and a volatility CRPS section.
{% endhint %}

## Additional Resources

* [Literature](https://github.com/crunchdao/crunch-synth/blob/master/LITERATURE.md)
* Useful Python [packages](https://github.com/crunchdao/crunch-synth/blob/master/PACKAGES.md)
* Synth: [Introducing Volatility CRPS](https://synthdata.co/research/introducing-volatility-crps-updated)

[^1]: Equivalent to 500 / 4.34524, see why [here](https://www.cuemath.com/questions/what-is-the-average-number-of-weeks-in-a-month/).

[^2]: Conversion rate is coming from taostats' API.

[^3]: Conversion rate is coming from Coingecko's API.
