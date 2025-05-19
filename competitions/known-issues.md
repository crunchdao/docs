---
description: A list of known problem and how to solve them.
---

# Known Issues

## `NaN` in the Cloud, but none locally

The `index` might not always be the same in the Cloud and locally as the data that is shared **is** different.

Depending on your code, this could cause some issues when trying to do operation between `pandas` objects that do not share the same `index`.

```python
# will use a range index, from 0 to len(X_test)
final_ensemble = pd.Series(
    [0] * len(X_test),
    dtype='float'
)

# pandas objects do not share the same index, it will likely result in only nans
final_ensemble += (X_test.loc[:, 'some_colomn'] * 2)
```

### Use the same `index`

```python
final_ensemble = pd.Series(
    [0] * len(X_test),
    index=X_test.index,
    dtype='float',
)
```

### Reset the `index`

```python
X_test.reset_index(inplace=True)

# then do your operations
final_ensemble += (X_test.loc[:, 'some_colomn'] * 2)
```

## CatBoostError: Can't create train working dir: catboost\_info error <a href="#catboosterror-cant-create-train-working-dir-catboost_info-error" id="catboosterror-cant-create-train-working-dir-catboost_info-error"></a>

{% hint style="success" %}
[This issue should be fixed automatically (by monkey patching the catboost library). ](https://github.com/crunchdao/crunch-cli/blob/5f91ecb9d041b12e940947f6214cc64d12bf37ad/crunch/monkey_patches.py#L117)
{% endhint %}

CatBoost create a directory for persisting his state. But the Run does not allow you to create file anywhere.

### Change the train directory to: `/tmp` <a href="#change-the-train-directory-to-tmp" id="change-the-train-directory-to-tmp"></a>

_If the state doesn't need to be persisted_, the `/tmp` directory is the way to go.

```python
model.set_params(train_dir='/tmp/catboost_info')
```

### Change the train directory to: `model_directory` <a href="#change-the-train-directory-to-model_directory" id="change-the-train-directory-to-model_directory"></a>

If the state does need to be persisted, store everything inside the `model_directory` as this folder will be reused for the Out-of-Sample phase.

```python
info_path = os.path.join(model_directory, 'catboost_info')
model.set_params(train_dir=info_path)
```

## Other

If your problem is not listed, don't hesitate to reach the team!
