---
description: A list of known problem and how to solve them.
---

# Known Issues

## `SCIPY_ARRAY_API` before importing `sklearn` or `scipy`

Since the [SciPy array API](https://docs.scipy.org/doc/scipy/dev/api-dev/array_api.html) is experimental, models that need it must enable it.

In a **dedicated cell** at the **very top**—even **before the imports**—and **without changing anything**, write the following:

{% code title="Notebook Cell (Python)" %}
```python
# @crunch/keep:on

import os
os.environ['SCIPY_ARRAY_API'] = '1'
```
{% endcode %}

## `NaN` in the Cloud, but none locally

The `index` might not always be the same in the Cloud and locally as the data that is shared **is** different.

Depending on your code, this could cause some issues when trying to do operation between `pandas` objects that do not share the same `index`.

{% code title="Notebook Cell (Python)" %}
```python
# will use a range index, from 0 to len(X_test)
final_ensemble = pd.Series(
    [0] * len(X_test),
    dtype='float'
)

# pandas objects do not share the same index, it will likely result in only nans
final_ensemble += (X_test.loc[:, 'some_colomn'] * 2)
```
{% endcode %}

### Use the same index

{% code title="Notebook Cell (Python)" %}
```python
final_ensemble = pd.Series(
    [0] * len(X_test),
    index=X_test.index,
    dtype='float',
)
```
{% endcode %}

### Reset the index

{% code title="Notebook Cell (Python)" %}
```python
X_test.reset_index(inplace=True)

# then do your operations
final_ensemble += (X_test.loc[:, 'some_colomn'] * 2)
```
{% endcode %}

## Other

If your problem is not listed, don't hesitate to reach the team!
