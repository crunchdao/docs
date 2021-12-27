---
description: How to get started with the DataCrunch Tournament
---

# 🐥 Getting Started

## Introduction



**DataCrunch is the first DAO-powered market neutral fund**.

The DataCrunch Tournament is where you predict the stock market using machine learning on financial data. Your models will earn rewards based on performance on live financial data.

## Summary

1. Sign up at [https://tournament.crunchdao.com/](https://tournament.crunchdao.com)
2. Download the dataset on a **weekly** basis
3. Build your model and submit your predictions
4. Get your **monthly $CRUNCH reward** based on the Global Leaderboard

## Data

DataCrunch provides its data scientist community free curated, high quality and obfuscated data.

There are - at the time of writing - 6 datasets currently in use in the Tournament

| Name     | Link                                                                      |
| -------- | ------------------------------------------------------------------------- |
| X\_train | [data/X\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv)  |
| y\_train | [/data/y\_train.csv](https://tournament.datacrunch.com/data/y\_train.csv) |
| X\_test  | [/data/X\_test.csv](https://tournament.datacrunch.com/data/X\_test.csv)   |

![Sample of X\_train dataset output](<../.gitbook/assets/image (17).png>)

Each `id` in `X_train` corresponds to a stock at a specific time `Moons`. The `Features` describe specific attributes of the stock at the time.

![Sample of y\_train targets](<../.gitbook/assets/image (18).png>)

The `y_train` file contains 3 targets `target_r`, `target_g`, `target_b` that correspond to the performance of the stock over 3 time horizons.

The frequency of the `Moons`, `target_r`, `target_g` and `target_b` can be found here: [https://tournament.crunchdao.com/inspect](https://tournament.crunchdao.com/inspect)

![Here is an example of Dataset gordon-geeko with a frequency of 1 month between Moons and performance of targets R/G/B over 30 days, 60 days and 90 days respectively](<../.gitbook/assets/image (19).png>)

{% hint style="info" %}
&#x20;The files are pretty heavy, please make sure your computer has available free space.
{% endhint %}

## Modeling

Your objective is to build a model to predict future targets using live features that correspond to the current stock market.

You will also find Notebooks on the official DataCrunch Github repository and Google Colaboratory including examples of:

* Baseline Pipeline
* Exploratory Data Analysis EDA
* Feature Engineering
* Cross Validation methods

{% embed url="https://github.com/datacrunch-com/datacrunch-notebooks" %}
DataCrunch Notebooks
{% endembed %}

{% embed url="https://colab.research.google.com/drive/1ZXARI_CQMbCWs-C_aXEtWPIDZVW61KJ4?usp=sharing" %}
Colab Getting Started Baseline Notebook
{% endembed %}

You can use any language or framework to build your model.&#x20;
