---
description: Data page description
---

# Data

## Dataset

CrunchDAO provides its data scientist community with free curated, high-quality but obfuscated data. This obfuscation is the only way to allow the community to work on institutional data.

| X\_train            | [/data/X\_train.csv](https://tournament.datacrunch.com/data/X\_train.csv)                      |
| ------------------- | ---------------------------------------------------------------------------------------------- |
| y\_train            | [/data/y\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv)                      |
| X\_test             | [/data/X\_test.csv](https://tournament.datacrunch.com/data/X\_test.csv)                        |
| example\_submission | [/data/example\_submission.csv](https://tournament.crunchdao.com/data/example\_submission.csv) |

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Sample of X_train dataset output</p></figcaption></figure>

### ID

Each `id` in `X_train and X_test` corresponds to a stock at a specific time `Moons`.&#x20;

### Moons

The frequency of the `Moons` is one week.

### Features

The `features` describe specific attributes of a stock at a point in time.

### Targets

The `y_train` file contains 4 targets `target_w`,`target_r`, `target_g`, `target_b` that correspond to the idiosyncratic return of the stock over 3-time horizons: 7, 30, 60, and 90 days respectively.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption><p>Sample of y_train targets</p></figcaption></figure>

### Split

The overall dataset is split in two: train and test. The test set starts one moon after the last moon of X\_train.

{% hint style="warning" %}
&#x20;Files might be big so make sure to have enough space before downloading.
{% endhint %}

##
