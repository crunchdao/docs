---
hidden: true
---

# Synth: Synthetic Price Data

## Overview

Enter Synth and submit full return density forecasts for selected assets.

**Trackers (models)** must generate returns density predictions, not single predictions and maximize accuracy overall forecasts. See open-source Crunch framewor&#x6B;**:** [Crunch-Synth](https://github.com/crunchdao/crunch-synth)

**Covered Assets:**

* Bitcoin (BTC)
* Ethereum (ETH)
* Solana (SOL)
* Tether Gold (XAUT)
* SP500 tokenized ETF (SPYX)
* NVIDIA tokenized stock (NVDAX)
* Tesla tokenized stock (TSLAX)
* Apple tokenized stock (AAPLX)
* Alphabet tokenized stock (GOOGLX)

## Rewards

* \[Period 1] - 1-month warmup period before Live Trading
* \[Period 2] - 3 months live with a fixed $30,000 USDC prize pool
* \[Period 3] - Rewards pools will be generated from mining Synth subnet and may be higher.

## Prediction Targets

Trackers must predict the **probability distribution of returns**, defined as:

$$
r_{t,k} = P_t - P_{t-k}
$$

For each defined step $$k$$ (e.g., 5 minutes, 1 hour, …), your tracker must return a full **probability density function (PDF)** over the future price change $$r_{t,k}$$.

## Visualize the challenge

The Synth coordinator is evaluated on **incremental return predictions**, not raw prices.\
Incremental returns capture the _relative_ change in price and produce a stationary series that is easier to model and compare across assets.

Below is an example of a **density forecast over incremental returns for the next 24h at 5-minute intervals**:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Below is a minimal example showing what your tracker might return:

```python
# Expected output
>>> model.predict(asset="SOL", horizon=86400, step=300)
[
    {
        "step": (k + 1) * step,
        "prediction": {
            "type": "builtin",
            "name": "norm",
            "params": {
                "loc": -0.01,       # mean return
                "scale": 0.4     # standard deviation of return
            }
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
* A player must wait **7 days** to get a meaningful ranking, since the leaderboard is based on a **7-day rolling average** of CRPS scores.
* Players may **enter or exit the game at any time**.
* Each player may run **two active models**, which can be updated at any time.

#### Prediction Phase <a href="#prediction-phase" id="prediction-phase"></a>

In each prediction round, players must submit **a set of** **density forecasts**.

A **prediction round** is defined by **one asset**, **one forecast horizon** and **one or more step resolutions**.

* A **24-hour horizon** forecast&#x20;
  * Triggered **hourly** for each asset
  * Step resolutions: **{5-minute, 1-hour, 6-hour, 24-hour}**
  *   Supported assets:

      ```python
      ["BTC", "SOL", "ETH", "XAUT", "SPYX", "NVDAX", "TSLAX", "AAPLX", "GOOGLX"]
      ```
* A **1-hour horizon** forecast&#x20;
  * Triggered **every 12 minutes** for each asset
  * Step resolutions: **{1-minute, 5-minute, 15-minute, 30-minute, 1-hour}**
  *   Supported assets:

      ```python
      ["BTC", "SOL", "ETH", "XAUT"]
      ```

All required forecasts for a prediction round must be generated **within 40 seconds**.

#### Scoring <a href="#scoring" id="scoring"></a>

* Once the full horizon has passed, each prediction is scored using a **CRPS scoring function**.
* A lower **CRPS score** reflects more accurate predictions.
* Players cannot skip predictions. Missing or invalid predictions receive the **worst CRPS score of the round**.

#### Relative Scoring <a href="#scoring" id="scoring"></a>

Leaderboard ranking is based on a **7-day rolling average** of CRPS scores across **all assets and horizons**, evaluated **relative to other participants**, for each prediction round:

* The **best CRPS score is assigned a normalized score of 1**
* The **worst 5% of CRPS scores are assigned** **a score of 0**
* Intermediate scores are scaled accordingly

### Leaderboard & Game State <a href="#game-state" id="game-state"></a>

The leaderboard displays several **relative performance indicators**, computed across **all assets and horizons**:

* **Anchor CRPS (7 days):** Rolling relative CRPS average over the past 7 days (primary ranking metric).
* **Steady CRPS (3 days):** Rolling relative CRPS average over the past 3 days.
* **Recent CRPS (24 hours):** Rolling relative CRPS average over the past 24 hours.

#### Payouts <a href="#payouts" id="payouts"></a>

* Rewards are distributed **weekly** and a fixed reward pool is allocated for each payout period.
* Rewards are distributed **proportionally to each model’s Anchor score** among **eligible models only**.
* **Eligibility condition:**\
  Only models that **outperform the benchmark model (synth:benchmarktracker) at the time of the payout** are considered for rewards.
  * Models with an Anchor score **below or equal to the benchmark at payout time** are included in calculations but receive no payout, leaving any fraction of the pool tied to lower-performing models undistributed.
  * The benchmark model is a reference model provided by the game and may evolve over time

#### Game Duration <a href="#game-duration" id="game-duration"></a>

* After the warm-up phase of **4 weeks** ends on **mid-February**, the official game begins with real rewards.
* The initial season will run for **3 months of live play** and the start of a new season.
* Game rules and mechanics may evolve with player feedback, particularly during the warm-up and the first season.

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
            "scale": y_var
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
* Output length: horizon // step

Each density prediction must comply with the [density\_pdf](https://github.com/microprediction/densitypdf/blob/main/densitypdf/__init__.py) specification.

{% hint style="info" %}
You can refer to the [Tracker examples](https://github.com/crunchdao/crunch-synth/tree/master/crunch_synth/examples) for guidance.
{% endhint %}

## Additional Resources

* [Literature](https://github.com/crunchdao/crunch-synth/blob/master/LITERATURE.md)
* Useful Python [packages](https://github.com/crunchdao/crunch-synth/blob/master/PACKAGES.md)
