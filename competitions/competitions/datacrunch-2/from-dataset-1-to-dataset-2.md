# From Dataset #1 to Dataset #2

## Datacrunch New Data Release ⍺H

With this released Datacrunch may come with one of the most significant dataset upgrades since the fund launched. ⍺H is built around the team internal strategy and represents a fundamental return to the source of the original competition setup: Simpler metric, longer datasets and one target to rule them all.

This upgrade brings the competition dataset into closer alignment with our internal production strategy. The changes, informed by extensive backtesting and confirmed under live trading, remove legacy constraints and consolidate around the signals that demonstrate the strongest and most stable correlation to returns. For participants, this means more transparent evaluation and a clearer understanding of what drives success.

## What's New in ⍺H

### Extended Historical Coverage: More data, More Signal

The dataset now reaches further back in time, capturing a wider range of market conditions. This matters because models trained only on recent data tend to overfit to the prevailing regime, whether that's a low-volatility bull market, a crisis period, or a recovery phase. By including more years of history, ⍺H as more memory and exposes models to recessions, rate cycles, sector rotations, and volatility spikes they might otherwise never encounter during training. The team's internal Benchmark’s results are more robust and hold up when market conditions inevitably shift.&#x20;

### Bigger Cross-Sectional Universe: More assets, more edge

Each weekly snapshot now contains more stocks than ever before. A larger cross-section strengthens the statistical foundation for relative rankings, when you're predicting which stocks will outperform, having more candidates creates cleaner separation between winners and losers. It also reduces the risk of overfitting to idiosyncratic behavior in a narrow universe. For a market-neutral strategy that relies on going long some stocks and short others, breadth is essential for constructing diversified portfolios that isolate alpha from market beta.

### Refined Features: Less is more

The 1,150 anonymized features have been reworked based on our internal Benchmark’s performance. Some features that seemed promising in backtests but failed to generate alpha in production have likely been removed or transformed. Others may have been re-scaled, combined, or orthogonalized to reduce redundancy. While participants still can’t identify exactly what each feature represents, the refinement improves signal-to-noise ratio and makes the feature space more tractable for machine learning models.

### One target survived: 28-Day Forward Returns

The prediction target has fundamentally changed. Rather than 4 return horizons, models need now to focus all their attention to forecast performance over a 28-day window. This longer horizon smooths out short-term noise and market microstructure effects, focusing instead on moves driven by fundamental revaluation. It also aligns better with the fund’s weekly rebalancing. Finally it avoids participants to compromise between horizons and focus all their attention to one and only one target. The four-moon embargo period directly corresponds to this target window, preventing any information leakage. As always only live performance will prevail.

### Dataset Architecture

The data maintains a weekly frequency, with each row representing a single stock at a given timestamp. Features are anonymized (1,150 in total) and describe each stock's relative state at that moment. The "moon" identifier marks time periods sequentially, while stock IDs are unique to each moon: the same company receives different IDs across different time periods.

The embargo period spans four moons, matching the target's forward-looking window.

## New Tournament Mechanics:

### No more submission Window

The submission window constraint is gone. Participants can submit whenever they want, and any run completed before Sunday at 12pm UTC counts for that week. Given the target's resolution timeline, scores appear five weeks after submission.

### A Stronger Reward Scheme Toward Top Performers

The payout structure now follows an aggressive power-law distribution. Below-median performers receive nothing, and above median, rewards scale with the 20th power of excess rank. To put this in perspective: a participant at the 75th percentile captures far less than someone at the 90th, who in turn captures far less than someone at the 99th. This concentration has strategic implications: incremental improvements near the top are worth dramatically more than climbing from average to good.&#x20;

### Staking incoming

As many of you requested the feature the Staking will arrive in Q1 2026. Participants will be able to put tokens behind their models, creating direct economic exposure tied to signal quality. The team views this skin-in-the-game mechanism as a great tool for trusting models faster under this new dataset and with larger capital allocations.

### Getting Started

New participants can begin with the provided quickstarter notebook, implementing the required `train` and `infer` functions and validating locally before submitting to the cloud environment. Weekly compute allocation stands at **15 hours** of GPU or CPU time, resetting each Sunday.
