---
description: Data page description
---

# 🗃 Data

## Dataset

CrunchDAO provides its data scientist community free curated, high quality and obfuscated data.

There are - at the time of writing - 6 datasets currently in use in the Tournament

| X\_train | [/data/X\_train.csv](https://tournament.datacrunch.com/data/X\_train.csv) |
| -------- | ------------------------------------------------------------------------- |
| y\_train | [/data/y\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv) |
| X\_test  | [/data/X\_test.csv](https://tournament.datacrunch.com/data/X\_test.csv)   |

![Sample of X\_train dataset output](<../.gitbook/assets/image (22).png>)

Each `id` in `X_train` corresponds to a stock at a specific time `Moons`. The `Features` describe specific attributes of the stock at the time.

![Sample of y\_train targets](<../.gitbook/assets/image (25).png>)

The `y_train` file contains 3 targets `target_r`, `target_g`, `target_b` that correspond to the performance of the stock over 3 time horizons.

The frequency of the `Moons`, `target_r`, `target_g` and `target_b` can be found here: [https://tournament.crunchdao.com/inspect](https://tournament.crunchdao.com/inspect).

![Here is an example of Dataset gordon-geeko with a frequency of 1 month between Moons and performance of targets R/G/B over 30 days, 60 days and 90 days respectively](<../.gitbook/assets/image (28) (1).png>)

{% hint style="warning" %}
&#x20;Files might be big (200+MB) so make sure to have enough space before downloading.
{% endhint %}

##
