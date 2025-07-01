# Data

In all Crunchs, the crunchers can access our datasets as follow:

## Splited datasets

### X\_train, y\_train, X\_test

1. `X_train`, `y_train`: Labeled training dataset;
2. `X_test`, `y_test`: Small portion of the test set that can be used to [run local test](./#local-testing).

{% hint style="info" %}
In Crunches, crunchers submit code or models in the form of notebooks or Python files. These submissions are then run on testing or live data by the system. As a result, crunchers never have direct access to the testing data.&#x20;
{% endhint %}

### The example prediction

A sample prediction is provided to show the required format for the output of the submission. Any deviation from this format will result in an invalid submission.

The [crunch-cli](https://pypi.org/project/crunch-cli) also uses this file to perform a local check.

The values are usually either random or a constant.

## Data Formats

Data can come in various formats

### Cross-sectional DataFrame

Containing only a `moon`, `id` and `feature`s columns.

{% hint style="info" %}
Moon are proxy for timestamps, in other words moons are date
{% endhint %}

Participant can access the name of the columns (the features) with [parameters of the code interface](code-interface.md#hidden-parameters).

#### How to load the data?

Loading the data from a notebook is really easy, the load\_data function will make sure that the latest version of the data is available locally, or download it if necessary, and return 3 dataframes.

```python
# X_train: pandas.DataFrame
# y_train: pandas.DataFrame
#  X_test: pandas.DataFrame

X_train, y_train, X_test = crunch.load_data()
```

#### Examples from the DataCrunch competition

<figure><img src="../../.gitbook/assets/datacrunch X_train.parquet.png" alt=""><figcaption><p>DataCrunch's <code>X_train.parquet</code></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/datacrunch y_train.parquet.png" alt=""><figcaption><p>DataCrunch's <code>y_train.parquet</code></p></figcaption></figure>

#### Prediction's format

{% hint style="info" %}
There should be one column per target.
{% endhint %}

<figure><img src="../../.gitbook/assets/datacrunch example_prediction_reduced.parquet.png" alt=""><figcaption><p>DataCrunch's <code>example_prediction_reduced.parquet</code></p></figcaption></figure>

### DAG

...or **D**irected **A**cyclic **G**raph data are distributed as a pickled `dict` of `pandas.DataFrame`.

#### How to load the data?

```python
# X_train: typing.Dict[str, pandas.DataFrame]
# y_train: typing.Dict[str, pandas.DataFrame]
#  X_test: typing.Dict[str, pandas.DataFrame]

X_train, y_train, X_test = crunch.load_data()
```

#### Examples from the ADIA Lab Causality Discovery competition

<figure><img src="../../.gitbook/assets/causality discovery X_train.pickle.png" alt=""><figcaption><p>Causality Discovery's <code>X_train.pickle</code></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/causality discovery y_train.pickle.png" alt=""><figcaption><p>Causality Discovery's <code>y_train.pickle</code></p></figcaption></figure>

#### Prediction's format

The main column is `example_id`, formatted as follows:

<pre><code><strong>&#x3C;dataset_id>_&#x3C;from_node>_&#x3C;to_node>
</strong></code></pre>

<figure><img src="../../.gitbook/assets/causality discovery example_prediction_reduced.parquet.png" alt=""><figcaption><p>Causality Discovery's <code>example_prediction_reduced.parquet</code></p></figcaption></figure>

### Stream

Crunch's Streams are iterator object that allows you to traverse through all the elements of a time serie one at a time.&#x20;

#### How to load the data?

```python
# X_train: typing.List[typing.Iterator[dict]]
#  X_test: typing.List[typing.Iterator[dict]]

X_train, X_test = crunch.load_streams()
```

#### Examples from the Mid+One competition

<figure><img src="../../.gitbook/assets/mid one X_train.parquet.png" alt=""><figcaption><p>Mid-One's <code>X_train.parquet</code></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/mid one y_train.parquet.png" alt=""><figcaption><p>Mid-One's <code>y_train.parquet</code></p></figcaption></figure>

#### Prediction's format

{% hint style="warning" %}
Stream competition have a different way of submitting the result. Please follow the [Stream Code Interface](code-interface.md#stream) instead.
{% endhint %}

<figure><img src="../../.gitbook/assets/mid one example_prediction_reduced.parquet.png" alt=""><figcaption><p>Mid-One's <code>example_prediction_reduced.parquet</code></p></figcaption></figure>

