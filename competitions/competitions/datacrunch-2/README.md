---
description: >-
  This weekly cross-sectional problem target the expected returns of the 3000
  most liquid US equities.
cover: ../../../.gitbook/assets/banner (9).webp
coverY: 0
---

# DataCrunch Equity Market Neutral #2

[DataCrunch](https://datacrunch.com/) uses the quantitative research of the CrunchDAO to manage its systematic market-neutral portfolio.

The long-term strategic goal of the fund is capital appreciation by capturing idiosyncratic return at low volatility.

In order to achieve this goal, DataCrunch needs the community to assess the relative performance of all assets in a subset of the [Russell 3000](https://www.investopedia.com/terms/r/russell_3000.asp) universe. In other words, DataCrunch is expecting your model to predict the performance of the constituent of its investment universe.

This dataset is the new of the original DataCrunch dataset that can be found [here](https://hub.crunchdao.com/competitions/datacrunch).

To read more about the evolutions between #1 and #2 of the DataCrunch datasets, please [read the full article](https://docs.crunchdao.com/competitions/competitions/datacrunch-2/from-dataset-1-to-dataset-2).

## Reward Scheme

DataCrunch is distributing 2,000 USDC every weeks.

The reward scheme is calculated as follows for both metrics:

{% code title="Pseudo code (Python flavored)" %}
```python
# Reward Calculation
weekly_rewards = 2000

# 0 = worst, 1 = best
percentile_rank = your_rank / nb_participants

if percentile_rank <= 0.5:
    reward = 0
else:
    # excess above median, scaled 0–1
    e = 2 * (percentile_rank - 0.5)
    weight = e ** 20
    reward = weekly_rewards * (weight / sum(participants_weights))
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image2.png" alt=""><figcaption></figcaption></figure>

All rewards are computed on the leaderboards.

The Historical Rewards are the sum of every payout you have received from DataCrunch.

The Projected Rewards are the current estimated rewards yet to be distributed.

## Dataset

Each row of the dataset represents a single stock at a given weekly timestamp.

### `X_train`

* `moon`: A sequentially increasing integer representing a date. Time between subsequent moons is constant, denoting a weekly fixed frequency at which the data is sampled.
* `id`: A unique identifier representing a stock at a given `moon`. Note that the same asset has a different `id` in different `moon`.
* `Feature_1`, …, `Feature_1150`: Anonymised features that describe the state of assets on a given `moon`. They are ways of assessing the relative performance of each stock on a given `moon`.

### `y_train`

* `moon`: Same as in `X_train`.
* `id`: Same as in `X_train`.
* `target`: The targets that will help you build your models. It is derived from the 28 days forward returns.

### `X_test` and `y_test`

`X_test` and `y_test` have the same structure as `X_train` and `y_train` but comprise only one `moon` at each iteration. These files are used to simulate the submission process locally via `crunch.test()` (within the code), or `crunch test` (via the CLI). The aim is to help participants debug their code and have successful submissions. A successful local test usually means no errors during execution on the submission platform.

### Embargo

The embargo is defined by the length of the target and is thus 4 moons.

## Tournament Structure

### Data Splits

The DataCrunch competition follows a fixed and transparent structure:

* Local data:
  * The first 15 years of the dataset. A smaller version of the latest 2.5 years of these 15 years is provided for small configurations.
* Cloud data:
  * A public Out-of-Sample allowing participants to test your submission in the cloud. This data is on the first two month of 2020 year.
  * A private Out-of-Sample allowing DataCrunch to have historical performance of the models in order to do meta-modelling and ensemble research.
  * The last available date of the dataset will be scored on a weekly basis, after the target is resolved.

{% hint style="info" %}
Your code will have access to the entire dataset when running in the cloud.
{% endhint %}

### Submission Cut-off

The window to submit and lock your model is removed. You can submit your code / model whenever you want. If the run completes before Sunday 12pm UTC, it will be taken into account for the week. Otherwise, the run will be terminated and ignored for the week.

The prediction target takes 1 day + 4 weeks to be fully resolved, so the score for a model submitted in week #1 will be available and published in week #6.

## Scoring and Evaluation

Participants are evaluated using the [Pearson correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) between their predictions and the target on the last date of the dataset.

## Computing Resources

Participants will be allocated a specified quantity of computing resources within the cloud environment for the execution of their code.

Participants are entitled to **15 hours** of GPU or CPU compute time per week. The competition being weekly, you will have to manage your weekly computing resources consumption to comply with this constraint.

Quota will be reset each Sunday at 12pm UTC.

## Getting Started

Participants can begin by implementing the required `train` and `infer` functions and validating locally using the [provided quickstarter notebook](https://colab.research.google.com/github/crunchdao/quickstarters/blob/master/competitions/datacrunch-2/quickstarters/quickstarter/quickstarter.ipynb).

The API is as follow:

{% code title="Python Notebook Cell" expandable="true" %}
```python
def train(
    X_train: pd.DataFrame,
    y_train: pd.DataFrame,
    model_directory_path: str,
    loop_moon: int,
    embargo: int,
) -> None:
    """
    At each retrain this function will have to save an updated version of the model under the model_directory_path, as in the example below.
    
    Note: You can use other serialization methods than joblib.dump(), as long as it matches what reads the model in infer().

    Args:
        X_train, y_train: the data to train the model.
        model_directory_path: the path to save your updated model.
        loop_moon: the moon currently being processed.
        embargo: data embrago.

    Returns:
        None: Returned value is ignored.
    """
```
{% endcode %}

{% code title="Python Notebook Cell" expandable="true" %}
```python
def infer(
    X_test: pd.DataFrame,
    model_directory_path: str,
    loop_moon: int,
    embargo: int,
) -> pd.DataFrame:
    """
    This function will load the model(s) saved at the previous iteration and use it/them to produce your inference on the current moon.
    It is mandatory to send your inferences with the ids and moon so the system can match it correctly.

    Args:
        X_test: the independant variables of the current moon passed to your model.
        model_directory_path: the path to the directory to the directory in wich we will be saving your updated model.
        loop_moon: the moon currently being processed.
        embargo: data embargo.

    Returns:
        A pd.DataFrame (moon, id, prediction) with the inferences of your model for the current moon.
    """
```
{% endcode %}

{% hint style="info" %}
Running a local test will validate your API. If it passes locally, it will also pass in the cloud environment.
{% endhint %}
