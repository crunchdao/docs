---
description: Can you outsmart the falcons and predict where the dove will go next?
---

# Falcon: The Collective Pricing Engine

## Overview

Enter a real-time forecasting game where players use probabilistic models to forecast the dove’s future location, based on the movement of the dove and the chasing falcons.

Submit forward density predictions to maximize accuracy and earn rewards!

It’s a blend of strategy, live data, and statistical forecasting.

## Data

Your model must process a sequence of records that will be received in real time.

Each record provides:

* `time`: The current time;
* `falcon_location`: A location of one falcon;
* `dove_location`: The current dove location;
* `falcon_id`: The falcon identity;
* `falcon_wingspan`: A figurative measure of dexterity, precision and aggression whose utility or lack thereof is up to you to decide;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The falcon locations are shown as colored dots on the figure above. Some falcons may provide useful information as to (track) the future location of the dove, or the uncertainty of the same, whereas others may not. Their utility, or otherwise, is for you to determine.

The game runs on live data, the feed will be available from Sunday 22:00 UTC to Friday 22:00 UTC.

### Expected Interruptions of the Data Feeds

Each day, around 21:00 UTC, there is a window lasting 5–30 minutes during which data may be delayed, unavailable or the market might behave different than during other times in the day. This is caused by the underlying data streams going into a rollover/maintenance period. Your model should be prepared to handle this change in the underlying data gracefully to not loose wealth!&#x20;

### Unexpected interruptions of the Data Feeds

As the game relies on real-time data occasional outages may occur. During these periods, models will not receive new inputs. Once the feed resumes, there will be a burn-in period where fresh data is collected before model performance is evaluated again. This ensures fairness and allows models to recalibrate to current market conditions.

## Game Rules

### Start

* Each player begins with a starting wealth of **1000**.
* The game opens with a **warmup phase** where predictions are scored, but wealth is reset at the beginning of each day —this is for players to calibrate their models. This phase will last for **2 weeks, until end of October.**
* Players can enter and exit the game at any time.
* Players have a single active model they can update at any time.

### Prediction Phase

* For each prediction round, players automatically invest a **fraction of their active wealth** into the pot.
* This amount is subtracted from their active wealth.
* The total pot is **inflated** slightly by a game-defined **inflation rate**.
* The model must **generate predictions in under 50 M**illiseconds.

### Scoring & Distribution

* Once the **true dove location** is revealed, each prediction is scored using a **likelihood function**.
* The pot is then distributed **proportionally** based on these likelihood scores.
* More accurate predictions earn a **larger share** of the pot.
* Player wealth will never go below 0.
* Players can skip predictions. Doing so means they cannot lose or gain wealth, as they are not participating in prize distribution. However, skipping has consequences:\
  their likelihood vintage resets to 0 and their likelihood is reset.

### Payouts

* When a player’s wealth exceeds a defined **wealth threshold of 2000**, they receive a **prize payout** equal to **10% of their wealth**.
* This payout is treated like a **withdrawal**: it’s subtracted from their active wealth and moved to **Realized Wealth**.
*   Realized Wealth is **distributed weekly**. A fixed pool of rewards is allocated, and each participant receives a share proportional to their earned wealth relative to the total earned by all players that week:

    $$
    \text{Payout}_i = \frac{W_i}{\sum_{j} W_j} \times R
    $$

### Game Duration

* After the 2-week warmup, the initial season runs for 3 months of live play. At the end, the winner is announced and a new season begins. Rules and mechanics may evolve with player feedback, especially after the first season.&#x20;

### Winning

* The player with the most Total Wealth (The sum of Active Wealth and Realized Wealth) wins the game.

## Game State

Explanation of the game state parameters displayed on the leaderboard for each player:

* **Total Wealth**: The sum of a player’s _Realized_ and _Active Wealth_.
* **Active Wealth**: The in-game capital currently at stake and used to generate returns.
* **Realized Wealth**: The portion of wealth that has been or will be paid out; no longer at risk or being used to generate returns.
* **Likelihood EWA**: The exponentially weighted average of ex-post log-likelihood scores over time.
* **Recent Likelihood**: The most recent ex-post log-likelihood of the player’s active model.
* **Longevity**: The total number of observations since the player joined the game.
* **Predicting**: Indicates whether the player’s model submitted predictions in the current round, can be Live / Idle.

## Probabilistic forecast

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
        "name": "normal",
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

### Mathematical Definition

The informal meaning of probabilistic forecast is a mixture of parametric univariate density functions where each is taken from a standard family (such as exponential, or gaussian).

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption><p>A mixture of gaussian densities conspire to match a fat-tailed distribution.</p></figcaption></figure>

### Engineering Definition

The meaning of probabilistic forecast is made more precise by means of the Python [density](https://github.com/microprediction/density) package which provides a dict specification of continuous univariate density function mixtures using the pydantic Python package. The function [validatedensitydict](https://github.com/microprediction/density/blob/main/density/validatedensitydict.py) will tell you whether or not your specification is valid.

## Create your Tracker

A tracker is a framework that processes real-time data to track the dove’s movement and predict its future location. It considers inputs like the dove’s position and falcon locations to generate a probabilistic forecast.

To create your tracker, you need to define a class that implements the `TrackerBase` interface. Specifically, your class must implement the following methods:

* `tick(self, payload: dict, performance_metrics: dict) -> None`\
  This method is called at every time step to process new payloads. Use this method to update your internal state or logic as needed.
* `predict(self) -> dict`\
  This method should return your prediction of the dove's location at a future time step. Ensure that the return format complies with the [density\_pdf](https://github.com/microprediction/densitypdf/blob/main/densitypdf/__init__.py) specification.

{% hint style="info" %}
You can refer to the [Tracker examples](https://github.com/microprediction/birdgame/tree/main/birdgame/examples) for guidance.
{% endhint %}

## Challenge your Tracker against the benchmark

To compare your Tracker's performance against the benchmark Tracker, use the `test_run` method provided in the `TrackerBase` class. This method evaluates your Tracker's efficiency over a series of time steps using [density\_pdf](https://github.com/microprediction/densitypdf/blob/main/densitypdf/__init__.py) scoring. A higher score reflects more accurate predictions.

## Additional Resources

* [Literature](https://github.com/microprediction/birdgame/blob/main/LITERATURE.md)
* Useful Python [packages](https://github.com/microprediction/birdgame/blob/main/PACKAGES.md)
