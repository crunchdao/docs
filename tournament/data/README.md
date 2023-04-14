---
description: Data page description
---

# Data

## Dataset

CrunchDAO provides its data scientist community with free curated, high-quality but obfuscated data. This obfuscation is the only way to allow the community to work on institutional data.



| Name                | CSV links                                                                                      | Parquet links                                                                                          | Comment                           |
| ------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | --------------------------------- |
| X\_train            | [/data/X\_train.csv](https://tournament.datacrunch.com/data/X\_train.csv)                      | [/data/X\_train.parquet](https://tournament.crunchdao.com/data/X\_train.parquet)                       |                                   |
| X\_train\_full      | [/data/X\_train\_full.csv](https://tournament.crunchdao.com/data/X\_train.csv)                 | [/data\_X\_train\_full.parquet](https://tournament.crunchdao.com/data/X\_train.parquet)                | Train set with unresolved targets |
| y\_train            | [/data/y\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv)                      | [/data/y\_train.parquet](https://tournament.crunchdao.com/data/y\_train.parquet)                       |                                   |
| X\_test             | [/data/X\_test.csv](https://tournament.datacrunch.com/data/X\_test.csv)                        | [/data/X\_test.parquet](https://tournament.crunchdao.com/data/X\_test.parquet)                         |                                   |
| example\_submission | [/data/example\_submission.csv](https://tournament.crunchdao.com/data/example\_submission.csv) | [/data/example\_submission.parquet](https://tournament.crunchdao.com/data/example\_submission.parquet) |                                   |

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Sample of X_train dataset output</p></figcaption></figure>

### ID

Each `id` in `X_train and X_test` corresponds to a stock at a specific time `Moons`.&#x20;

### Moons

The frequency of the `Moons` is one week.

### Features

In the realm of investment `strategies`, several methods exist to generate alpha. Below are the ones that are used in our `dataset`:

* **`cmechanics`**: Trend-following approach that seeks to identify and capitalize on trends in idiosyncratic returns and volatility. Its aim is to generate alpha by following these trends.
* **`ekinetic`:** This momentum outlook aims to systematically isolate and harvest excess returns arising from behavioral market anomalies by investing in diversification, not performance.
* **`gordon`**: The gordon strategy utilizes trade information from top management and senior executives based on academic research, suggesting that they possess alpha over other investors.
* **`dolly`**: Portfolio managers invest a significant amount of time and resources in identifying equities that will outperform the market in the long term (i.e., alpha). In Dolly, the community leverages machine learning to select top long-term asset managers and piggyback their trades.&#x20;
* **`3b1-signal`:** Institutional investors are leveraging equity factor risk models (Sector / Country Stock etc.) to predict return and hedge their bets. We investigate the extent to which nonlinearities not captured by standard linear models within equity factor risk models are present.
* **`vratios`**: Leverages fundamental data extracted from financial statements to build features that inform investment decisions.
* **`wrythm`:** Factors explaining cross-section returns in a linear way.

### Targets

The `y_train` file contains 4 targets `target_w`,`target_r`, `target_g`, `target_b` that correspond to the return of the stock over 4-time horizons: 7, 30, 60, and 90 days respectively.

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p>Sample of y_train targets</p></figcaption></figure>

### Split

The overall dataset is split in two: train and test. The test set starts one moon after the last moon of X\_train.

{% hint style="warning" %}
&#x20;Files might be big so make sure to have enough space before downloading.
{% endhint %}
