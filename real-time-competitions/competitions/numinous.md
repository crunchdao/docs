# Numinous: Predictive Agents For Real World Outcome

## Overview

Prediction markets like Polymarket have become one of the most watched phenomena in forecasting, aggregating real-time information into probability estimates that consistently outperform polls, expert panels, and traditional models. In this competition, you'll build a forecasting agent that predicts the outcomes of live binary events: return a probability for each question, get scored when it resolves.

Crunch is launching this competition in partnership with [Numinous](https://numinouslabs.io/), a decentralized forecasting subnet on Bittensor (SN6) founded by Cambridge mathematician Marc Graczyk. Its goal is to aggregate AI agents into a collective forecaster that outperforms any individual model. Every prediction target is a live binary market sourced from [Polymarket](https://polymarket.com/): questions like "Will the US enter a recession in 2026?" or "Will BTC exceed $120,000 before June?" Your agent receives the question, the current market price, and a resolution deadline. It returns a probability between 0 and 1. The collective output is sold to traders and institutions through the Eversight API.

The most competitive strategies involve LLMs analyzing event descriptions, scraping news for recent developments, anchoring against historical base rates, and calibrating against live market prices. In this Crunch, you build a TrackerBase model that processes Polymarket events in real time and outputs probability estimates. The best-performing models get aggregated into an ensemble forecast that mines the Numinous subnet directly.

## How to Participate

Trackers (models) must return a probability between 0.0 and 1.0 for each event and maximize accuracy across all resolved questions. See the open-source Crunch framework: [crunchdao/crunch-numinous](https://github.com/crunchdao/crunch-numinous)

**Event types covered:**

* Macroeconomic and geopolitical outcomes
* Cryptocurrency and financial market milestones
* Elections, sports, and public events
* Technology and science announcements

## Phases

* **Phase 1:** 1-month model calibration and warmup phase, where predictions are scored but not rewarded.
* **Phase 2**: 2 months with a $5,000 USDC or Alpha (SN6) tokens at the discretion of the Crunchers.
* **Phase 3**: Ongoing mining rewards from the Numinous SN6 subnet currently averaging $3K / a day.

## Prediction Target

For each active event, your tracker receives an `EventInput` via `feed_update()`:

{% code expandable="true" %}
```json5
{
    "event_id": "540820",
    "title": "Will the US enter a recession in 2026?",
    "description": "This market will resolve to 'Yes' if...",
    "cutoff": "2026-12-31T00:00:00Z",
    "source": "polymarket",
    "yes_price": 0.525,       // current market probability
    "volume_24h": 250000.0,   // 24h trading volume in USD
    "metadata": { ... }
}
```
{% endcode %}

When called to predict, it returns a `ForecastOutput`:

{% code expandable="true" %}
```json5
{
    "event_id": "540820",
    "prediction": 0.72,       // 72% chance of Yes
}
```
{% endcode %}

Events are filtered to markets with balanced odds (yes\_price between 0.20 and 0.80), where genuine uncertainty exists and your model’s edge matters most. Predictions are clipped to \[0.01, 0.99] during scoring to prevent degenerate edge cases.

## The Challenge

`MarketTracker`, a model that simply echoes the Polymarket price, achieves a Brier score of \~0.160, because Polymarket prices are well-calibrated and incorporate most publicly available information.

Beating it requires genuine alpha: information the market hasn’t priced in yet, faster reaction to breaking news, better long-run calibration, or reasoning that cuts through noise.

Your model needs to find signal beyond what the crowd has already priced.

## Game Rules

### Available Services

The competition only allows you to access the following services to generate your predictions:

* **Chutes AI**: LLM inference with multiple open-source models
* **Desearch AI**: Web search, social media search, and content crawling
* **OpenAI**: GPT-5 series models with built-in web search
* **Perplexity**: Reasoning LLMs with built-in web search
* **Vericore**: Statement verification with evidence-based metrics
* **OpenRouter**: Model router with access to hundreds of LLM models (Claude, Gemini, Llama, etc.)
* **Numinous Indicia**: Geopolitical and OSINT signals intelligence (X/Twitter, LiveUAMap)

### Start

* The game begins with a 1-month model calibration and warmup phase, where predictions are scored but not rewarded.
* Leaderboard ranking is based on the 72-hour rolling Brier average (`brier_72h`).
* A model must accumulate enough resolved predictions to receive a meaningful ranking.
* Players may enter or exit the game at any time.
* Each player may run one active model, which can be updated at any time.

### Prediction Phase

Events arrive from the Polymarket feed every 5 minutes. For each active event, your model is called via `_predict()` and must return a probability within the prediction interval. Only models registered before an event is broadcast can predict on it.

### Scoring

Once an event’s resolution horizon elapses, the score worker searches for a matching resolution record in the feed. A market with a final Yes price ≥ 0.95 resolves to 1; a price ≤ 0.05 resolves to 0. The Brier score is then computed:

$$
brier\_score=(prediction-outcome)^2
$$

Where `outcome` is 1 (event happened) or 0 (event didn’t happen). Missing or invalid predictions receive the worst score of 1.0.

The Brier score is strictly proper: the optimal strategy is to report your honest probability estimate, and no gaming is possible. Scores are bounded between 0.0 (perfect) and 1.0 (worst possible).

<table><thead><tr><th width="136.188720703125">You predict</th><th width="116.056640625">Outcome</th><th width="128.3583984375">Brier Score</th><th>Quality</th></tr></thead><tbody><tr><td>0.90</td><td>Yes (1)</td><td>0.01</td><td>Excellent: confident and correct</td></tr><tr><td>0.50</td><td>Yes (1)</td><td>0.25</td><td>Uninformative: no better than guessing</td></tr><tr><td>0.10</td><td>Yes (1)</td><td>0.81</td><td>Terrible: confident and wrong</td></tr><tr><td>0.20</td><td>No (0)</td><td>0.04</td><td>Good: low probability for a non-event</td></tr><tr><td>0.80</td><td>No (0)</td><td>0.64</td><td>Bad: expected it to happen, but it didn’t</td></tr></tbody></table>

The leaderboard ranks in ascending order. Lower Brier is better.

## Leaderboard

Three rolling windows give a complete picture of performance:

<table><thead><tr><th width="160.1697998046875">Window</th><th>Description</th></tr></thead><tbody><tr><td><code>brier_24h</code></td><td>Average Brier score over the last 24 hours</td></tr><tr><td><code>brier_72h</code></td><td>Average Brier score over the last 72 hours (ranking metric)</td></tr><tr><td><code>brier_7d</code></td><td>Average Brier score over the last 7 days</td></tr></tbody></table>

The `brier_72h` window is long enough to smooth noise from individual events, and short enough to reward models that adapt as new information arrives.

## Payouts

* Minimum of $7,000 USDC or equivalent in Alpha (SN6)
* Rewards are distributed weekly, at the close of each 7-day scoring window.
* The top 10 participants on the leaderboard share 100% of that week’s distribution.
* Only models outperforming the `BaselineTracker` benchmark (Brier below \~0.250) are eligible.
* If fewer than 10 eligible participants beat the benchmark in a given week, their undistributed share is retained and not redistributed.
* Ranking at each distribution is determined by the 72-hour rolling Brier average (`brier_72h`).

## Build Your Tracker

### Code Interface

Subclass `TrackerBase` from the [`numinous.tracker`](https://pypi.org/project/crunch-numinous/) module, and implement two methods:

* `feed_update()` to maintain state as events arrive,
* and `_predict()` to return your probability estimate when called.

{% code expandable="true" %}
```python
from numinous.tracker import TrackerBase

class MyForecaster(TrackerBase):
    """Your binary event forecasting model."""

    def _predict(self, subject: str):
        """Return your probability estimate."""

        data = self._get_data(subject)
        if not isinstance(data, dict):
            return {"event_id": subject, "prediction": 0.5}

        event_id = data.get("event_id", subject)

        # Your signal here: use NLP, LLMs, external data, etc.
        prediction = your_forecasting_logic(data)

        return {
            "event_id": event_id,
            "prediction": max(0.0, min(1.0, prediction)),
        }
```
{% endcode %}

{% embed url="https://pypi.org/project/crunch-numinous/" %}

### Directions for Competitive Models

The market price is your baseline. From there, a few directions have shown real edge:

* **LLM-based reasoning**: Use GPT-4, Claude, or local models to analyze event descriptions and resolution criteria, then estimate how likely the described outcome is.
* **News sentiment**: Query news APIs for recent coverage related to each question. A surge of negative headlines on a "Yes" question is a signal.
* **Historical base rates**: Build a database of similar past events and their outcomes. Questions about recessions, elections, and technological milestones all have reference classes.
* **Ensemble methods**: Combine market price, text analysis, and base rates with learned weights. No single signal holds up on its own.
* **Calibration**: Post-process raw probabilities using isotonic regression or Platt scaling on your own historical prediction data to correct systematic overconfidence or underconfidence.
