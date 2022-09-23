---
description: Data page description
---

# Data

## Dataset

CrunchDAO provides its data scientist community free curated, high quality and obfuscated data.

There are - at the time of writing - 6 datasets currently in use in the Tournament

| X\_train | [/data/X\_train.csv](https://tournament.datacrunch.com/data/X\_train.csv) |
| -------- | ------------------------------------------------------------------------- |
| y\_train | [/data/y\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv) |
| X\_test  | [/data/X\_test.csv](https://tournament.datacrunch.com/data/X\_test.csv)   |

![Sample of X\_train dataset output](<../.gitbook/assets/image (22).png>)

### ID

Each `id` in `X_train and X_test` corresponds to a stock at a specific time `Moons`.&#x20;

### Moons

The frequency of the `Moons depends on the dataset :`&#x20;

* gordon-geeko : 30 days interval between each moon
* dolly : 90 days interval&#x20;
* e-kinetic : 7 days interval&#x20;
* c-mechanics : 7 days interval
* b-volatility : 7 days interval
* 3b1-signal : 7 days interval

### Features

The `features` describe specific attributes of a stock at a point in time.

### Targets

![Sample of y\_train targets](<../.gitbook/assets/image (25).png>)

The `y_train` file contains 3 targets `target_r`, `target_g`, `target_b` that correspond to the idiosyncratic return of the stock over 3 time horizons : 30, 60 and 90 days respectively.

### Split

The overall dataset is splitted in two : train and test. The test set start one moon after the last moon of X\_train.

{% hint style="warning" %}
&#x20;Files might be big (200+MB) so make sure to have enough space before downloading.
{% endhint %}

##
