# Code Interface

## Requirement

Your submission needs to provide at least three components: `import`s, `train()`, and `infer()`.

1. **`import`s**: As with any script, if your solution has dependencies on external packages be sure to import them. The system will automatically install your dependencies. Make sure that you only use packages that are [whitelisted](https://hub.crunchdao.com/competitions/datacrunch/submit/libraries).
2. **`train()`**: In the training function, users build and train the model to make inferences on the test data. The model must be stored in the `resources/` directory.
3. **`infer()`**: In the inference function, the trained model is loaded and used to make inferences on a sample of data that matches the characteristics of the training test.

{% hint style="info" %}
Some competitions may require more or fewer functions.

Always read the competition's documentation.
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
    has_gpu: bool,
    embargo: int,
    my_custom_value=42,  # user specified
) -> None

def infer(
    X_test: pandas.DataFrame,
    model_directory_path: str,
) -> pandas.DataFrame
```

## Containers

Some competitions will provide special objects as parameters that have unique behaviors of which you should be aware of.

#### Iterable vs Iterator

An iterator can be iterated many times, whereas an iterator can only be consumed once. ([learn more](https://stackoverflow.com/a/18809506/7292958))

The difference is subtle, but really important:

* The `train()` function is called once, but can **consume the streams as many times as necessary**
* The `infer()` function is called **only once per stream**, and there is no going back

{% hint style="info" %}
This specifically target the [Structural Break](../competitions/adia-lab-structural-break-challenge.md) competition.
{% endhint %}
