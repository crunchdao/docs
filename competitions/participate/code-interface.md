# Code Interface

## Requirement

Your submission needs to provide at least three components: `import`s, `train()`, and `infer()`.

1. **`import`s**: As with any script, if your solution has dependencies on external packages be sure to import them. The system will automatically install your dependencies. Make sure that you only use packages that are [whitelisted](https://hub.crunchdao.com/competitions/datacrunch/submit/libraries).
2. **`train()`**: In the training function, users build and train the model to make inferences on the test data. The model must be stored in the `resources/` directory.
3. **`infer()`**: In the inference function, the trained model is loaded and used to make inferences on a sample of data that matches the characteristics of the training test.

{% hint style="info" %}
[Request a package to be whitelisted.](whitelisted-libraries.md#requesting-a-package)
{% endhint %}

### Dynamic Parameters

If required, parameters can also be queried by name:

* If the name does not exist, `None` is used.
* If a default value is specified, the value is retained (useful for local testing).
* Typing is always ignored, so make sure it is correct.

They can be used in both the `train()` and the `infer()` functions:

```python
def train(
    X_train: pandas.DataFrame,
    y_train: pandas.DataFrame,
    model_directory_path: str,
    id_column_name: str,
    target_column_name: str,
    has_gpu: bool,
    embargo: int,
    my_custom_value=42,  # user specified
) -> None

def infer(
    X_test: pandas.DataFrame,
    model_directory_path: str,
    id_column_name: str,
    prediction_column_name: str,
) -> pandas.DataFrame
```

## Cross-Sectionnal and DAG

### Function Signature <a href="#function-signature" id="function-signature"></a>

```python
def train(
    X_train: pandas.DataFrame,
    y_train: pandas.DataFrame,
    model_directory_path: str
) -> None

def infer(
    X_test: pandas.DataFrame,
    model_directory_path: str
) -> pandas.DataFrame
```

### Available parameters

<table><thead><tr><th width="284">Parameter Name</th><th>Description</th></tr></thead><tbody><tr><td><code>number_of_features</code></td><td>the number of features in the dataset</td></tr><tr><td><code>model_directory_path</code></td><td>the path to the directory in wich your model should be saved into/loaded from</td></tr><tr><td><code>id_column_name</code></td><td>the name of the id column</td></tr><tr><td><code>moon_column_name</code></td><td>the name of the moon column</td></tr><tr><td><code>target_column_name</code></td><td>the name of the target column</td></tr><tr><td><code>prediction_column_name</code></td><td>the name of the prediction column</td></tr><tr><td><code>moon</code></td><td>the moon currently being processed</td></tr><tr><td><code>current_moon</code></td><td>same as moon</td></tr><tr><td><code>embargo</code></td><td>data embrago</td></tr><tr><td><code>has_gpu</code></td><td>if the runner has a gpu</td></tr><tr><td><code>has_trained</code></td><td>if the moon will train</td></tr></tbody></table>

## Stream

### Function Signature <a href="#function-signature" id="function-signature"></a>

```python
def train(
    streams: typing.List[typing.Iterable[crunch.StreamMessage]],
    model_directory_path: str
) -> None

def infer(
    stream: typing.Iterator[crunch.StreamMessage],
    model_directory_path: str
) -> typing.Generator[float]
```

{% hint style="info" %}
[An example Quickstarter is available.](https://github.com/crunchdao/quickstarters/blob/master/generic/stream/empty/main.py)
{% endhint %}

{% hint style="warning" %}
The train function will only be called if the `resources/` directory is empty.
{% endhint %}

#### Iterable vs Iterator

An iterator can be iterated many times, whereas an iterator can only be consumed once. ([learn more](https://stackoverflow.com/a/18809506/7292958))

The difference is subtle, but really important:

* The `train()` function is called once, but can **consume the streams as many times as necessary**
* The `infer()` function is called **only once per stream**, and there is no going back

### Available parameters

The system has a lot of hidden parameters that the user can use.

<table><thead><tr><th width="284">Parameter Name</th><th>Description</th></tr></thead><tbody><tr><td><code>model_directory_path</code></td><td>the path to the directory to the directory in wich we will be saving your updated model</td></tr><tr><td><code>has_gpu</code></td><td>if the runner has a gpu</td></tr></tbody></table>

## Spatial

### Function Signature <a href="#function-signature" id="function-signature"></a>

```python
def train(
    data_directory_path: str,
    model_directory_path: str
) -> None

def infer(
    data_file_path: str,
    model_directory_path: str
) -> pandas.DataFrame
```

### Available parameters

The system has a lot of hidden parameters that the user can use.

<table><thead><tr><th width="284">Parameter Name</th><th>Description</th></tr></thead><tbody><tr><td><code>data_directory_path</code></td><td>the path to the directory where the data is located</td></tr><tr><td><code>model_directory_path</code></td><td>the path to the directory to the directory in wich we will be saving your updated model</td></tr><tr><td><code>target_names</code></td><td>name of the targets to predict, usually one file per target is provided in the data directory</td></tr><tr><td><code>has_gpu</code></td><td>if the runner has a gpu</td></tr><tr><td><code>has_trained</code></td><td>if the train function has been called before</td></tr></tbody></table>

#### Infer only parameters

<table><thead><tr><th width="284"></th><th></th></tr></thead><tbody><tr><td><code>data_file_path</code></td><td>the path to the data that must be used to predict the target</td></tr><tr><td><code>target_name</code></td><td>the name of the current target being predicted</td></tr></tbody></table>

## Global variables

[Global variables being commented out is a known issue.](../faqs/known-issues.md#global-variables)
